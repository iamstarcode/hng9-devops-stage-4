
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
                withDockerRegistry([ credentialsId: 'dockerhub', url: '' ]) {
                    dockerImage.push()
                }
            }

            stage('Cleaning Up') {
                steps {
                sh "docker rmi --force $registry:$BUILD_NUMBER"
                }
            }
    }
}
