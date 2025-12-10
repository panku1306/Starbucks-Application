pipeline {
    agent any

    tools {
        jdk 'jdk17'
        nodejs 'node'
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
                script {
                    dockerImage = docker.build("starbucks")
                }
            }
        }

        stage("Tag & Push to DockerHub") {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'docker') {
                        dockerImage.push("latest")
                    }
                }
            }
        }
    }
}
