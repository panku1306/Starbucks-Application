pipeline {
    agent any

    tools {
        jdk 'jdk17'
        nodejs 'node'   // Make sure Jenkins has a NodeJS installation named 'node'
    }

    stages {

        stage("Clean Workspace") {
            steps {
                cleanWs()
            }
        }

        stage("Git Checkout") {
            steps {
                git branch: 'main', url: 'https://github.com/panku1306/Starbucks-Application.git'
            }
        }

        stage("Install NPM Dependencies") {
            steps {
                sh "npm install"
            }
        }

        stage("Build Docker Image") {
            steps {
                sh "docker build -t starbucks ."
            }
        }

        stage("Tag & Push to DockerHub") {
            steps {
                script {
                    withCredentials([usernamePassword(
                        credentialsId: 'docker-creds',
                        usernameVariable: 'DOCKER_USERNAME',
                        passwordVariable: 'DOCKER_PASSWORD'
                    )]) {

                        sh """
                            echo \$DOCKER_PASSWORD | docker login -u \$DOCKER_USERNAME --password-stdin
                            docker tag starbucks \$DOCKER_USERNAME/starbucks:latest
                            docker push \$DOCKER_USERNAME/starbucks:latest
                        """
                    }
                }
            }
        }
    }
}
