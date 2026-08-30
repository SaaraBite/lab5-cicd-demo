pipeline {
    agent any

    environment {
        registry = "saarabait/lab5-cicd-demo"
        registryCredential = 'dockerhub'
        dockerImage = ''
    }

    stages {
        stage('Cloning Git') {
            steps {
                git 'https://github.com/SaaraBite/lab5-cicd-demo.git'
            }
        }

        stage('Building Docker Image') {
            steps {
                script {
                    dockerImage = docker.build registry
                }
            }
        }

        stage('Security Scan') {
            steps {
                script {
                    sh 'docker run --rm -v /var/run/docker.sock:/var/run/docker.sock aquasec/trivy:latest image ${registry}'
                }
            }
        }

        stage('Deploying Image') {
            steps {
                script {
                    docker.withRegistry('', registryCredential) {
                        dockerImage.push()
                    }
                }
            }
        }

        stage('Clean up') {
            steps {
                sh "docker rmi ${registry}"
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
