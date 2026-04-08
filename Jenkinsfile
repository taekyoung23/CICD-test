pipeline {
    agent any

    environment {
        strDockerImage = "kimtaekyoung/cicd-test:0.1"
    }

    stages {
        stage('Github Pull') {
            steps {
                git branch: 'main', url: 'https://github.com/taekyoung23/CICD-test.git'
            }
        }

        stage('Docker Image Build') {
            steps {
                script {
                    oDockImage = docker.build(strDockerImage, "-f Dockerfile .")
                }
            }
        }

        stage('Deploy Server') {
            steps {
                sshagent(credentials: ['Deploy-Privatekey']) {
                    sh 'scp -o StrictHostKeyChecking=no index.html ubuntu@54.180.228.177:/var/www/html/'
                }
            }
        }
    }
}