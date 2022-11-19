
pipeline {
    environment {
        registry = 'iamstarcode/dockerized-react'
        registryCredential = 'dockerhub'
        dockerImage = ''
    }

    agent any

    tools { nodejs 'Nodjs16' }

    stages {
            stage('Unit Tests') {
            steps {
                    script {
                        sh 'npm install --verbose'
                        sh 'npm test -- --watchAll=false'
                    }
            }
            }

            stage('Building Docker Image') {
                steps {
                    script {
                        dockerImage = docker.build registry + ':latest'
                    }
                }
            }

            stage('Deploying Docker Image to Dockerhub') {
                steps {
                    script {
                        docker.withRegistry('https://registry.hub.docker.com', registryCredential) {
                        dockerImage.push()
                        }
                    }
                }
            }

            stage('Cleaning Up') {
                steps {
                sh "docker rmi --force $registry:$BUILD_NUMBER"
                }
            }

        stage('Deploying') {
                steps {
                sh 'docker run --name mynginx3 -p 80:80 -d iamstarcode/dockerized-react:latest'
                }
        }
    }
}
