pipeline{
    agent any

    tools{
        maven 'maven'
    }

    stages{
        stage('git checkout'){
            steps{
                 git branch: 'main', url: 'https://github.com/eugenekofi/web-app.git'
            }
        }

        stage('clean and package'){
            steps{
                sh 'mvn clean package'
            }
        }

        stage('code analysis'){
            environment{
                ScannerHome = tool 'sonar'
            }
            steps{
                script{
                    withSonarQubeEnv('sonar'){
                      sh "${ScannerHome}/bin/sonar-scanner -Dsonar.projectKey=eugenekofi-webapp"  
                    }
                }
            }
        }

        stage('nexus uploads'){
            steps{
               nexusArtifactUploader artifacts: [[artifactId: 'maven-web-application', classifier: '', file: '/var/lib/jenkins/workspace/jomacs-webapp-pipeline/target/web-app.war', type: 'war']], credentialsId: 'nexus-credentials1', groupId: 'com.mt', nexusUrl: '54.227.88.161:8081/repository/jomacs-webapp/', nexusVersion: 'nexus3', protocol: 'http', repository: 'jomacs-webapp', version: '3.8.1-RELEASE' 
            }
        }

        stage('deploy to prod'){
            steps{
           deploy adapters: [tomcat9(credentialsId: 'tomcat-credentials1', path: '', url: 'http://3.85.39.18:8080')], contextPath: null, war: 'target/web-app.war'       
            }
        }
    }
    post {
        success {
            slackSend channel: 'eudev', color: 'good', message: "Build successful: ${currentBuild.fullDisplayName}"
        }
        failure {
            slackSend channel: 'eudev', color: 'danger', message: "Build failed: ${currentBuild.fullDisplayName}"
        }
        aborted {
            slackSend channel: 'eudev', color: 'warning', message: "Build aborted: ${currentBuild.fullDisplayName}"
        }
    }
}