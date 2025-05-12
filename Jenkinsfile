pipeline {
    agent any

    environment {
        IMAGE_NAME = "zhass/cicd-pipeline"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build App') {
            steps {
                sh 'chmod +x scripts/build.sh && ./scripts/build.sh'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'chmod +x scripts/test.sh && ./scripts/test.sh'
            }
        }

        stage('Docker Build') {
            steps {
                sh "docker build -t $IMAGE_NAME:${env.BUILD_NUMBER} ."
                sh "docker tag $IMAGE_NAME:${env.BUILD_NUMBER} $IMAGE_NAME:latest"
            }
        }

        stage('Docker Push') {
            steps {
                withDockerRegistry(credentialsId: 'docker_hub_id_creds', url: '') {
                    sh "docker push $IMAGE_NAME:${env.BUILD_NUMBER}"
                    sh "docker push $IMAGE_NAME:latest"
                }
            }
        }
    }
}

