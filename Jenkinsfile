pipeline {
    agent any

    environment {
        // Replace with your DockerHub repo and AWS details
        DOCKER_IMAGE = "yourdockerhubusername/jenkins-project"
        AWS_EC2_IP = "your.aws.ec2.ip"
        SSH_CREDENTIALS_ID = "your-jenkins-ssh-id" // Set up this ID in Jenkins
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/ankurbhardwajkiet/jenkins-projects.git'
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t ${DOCKER_IMAGE}:latest .'
                }
            }
        }

        stage('Push Docker Image to DockerHub') {
            steps {
                script {
                    withDockerRegistry([credentialsId: 'dockerhub-credentials-id', url: '']) {
                        sh 'docker push ${DOCKER_IMAGE}:latest'
                    }
                }
            }
        }

        stage('Deploy to AWS EC2') {
            steps {
                sshagent(['${SSH_CREDENTIALS_ID}']) {
                    sh """
                    ssh -o StrictHostKeyChecking=no ec2-user@${AWS_EC2_IP} << EOF
                    docker pull ${DOCKER_IMAGE}:latest
                    docker stop jenkins-project || true
                    docker rm jenkins-project || true
                    docker run -d --name jenkins-project -p 80:80 ${DOCKER_IMAGE}:latest
                    EOF
                    """
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment Successful!'
        }
        failure {
            echo 'Deployment Failed!'
        }
    }
}
