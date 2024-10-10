pipeline{
    agent any

    tools{
        maven 'maven'
    }

    stages{
        stage('git checkout'){
            steps{
                 git branch: 'main', url: 'https://github.com/eugenekofi/web-app.git',
            }
        }

        stage('maven build'){
            steps{
                sh 'mvn clean package'
            }
        }
    }
}