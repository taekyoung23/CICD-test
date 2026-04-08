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
                    def oDockImage = docker.build(strDockerImage, "-f Dockerfile .")
                }
            }
        }

        stage('Docker Image Push') {
            steps {
                script {
                    def oDockImage = docker.image(strDockerImage)
                    docker.withRegistry('', 'docker-auth') {
                        oDockImage.push()
                    }
                }
            }
        }

        stage('Deploy Server') {
            steps {
                sshagent(credentials: ['Deploy-Privatekey']) {
                    sh "ssh -o StrictHostKeyChecking=no ubuntu@54.180.228.177 docker container rm -f sampleweb || true"
                    sh "ssh -o StrictHostKeyChecking=no ubuntu@54.180.228.177 docker run -d -p 80:80 --name sampleweb ${strDockerImage}"
                }
            }
        }
    }
}