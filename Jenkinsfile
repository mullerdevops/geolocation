pipeline {
    agent any
    tools{
        maven 'M2_HOME'
    }
     environment {
    registry = '068920402932.dkr.ecr.us-east-1.amazonaws.com/geolocation_ecr_rep'
    dockerimage = ''
  }
    stages {
        stage('Checkout'){
            steps{
                git branch: 'main', url: 'https://github.com/mullerdevops/geolocation.git'
            }
        }
        stage('Code Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
          // Building Docker images
        stage('Building image') {
            steps{
                script {
                    dockerImage = docker.build registry
                }
            }
        }
        // Uploading Docker images into AWS ECR
        stage('Pushing to ECR') {
            steps{
                script {
                    sh 'aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 068920402932.dkr.ecr.us-east-1.amazonaws.com'
                    sh 'docker push 068920402932.dkr.ecr.us-east-1.amazonaws.com/my-docker-repo:latest'
                }
            }
        }
    }
}