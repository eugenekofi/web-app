pipeline{
    agent any

    tools{
        maven 'maven'
    }

    stages{
        stage('git checkout'){
            steps{
                 git branch: 'main', url: 'https://github.com/eugenekofi/web-app.git',changelog: true, poll: true
            }
        }
    }
}