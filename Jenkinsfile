pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                git url: 'https://github.com/username/project.git', branch: 'main'
            }
        }

        stage('Build') {
            steps {
                echo 'Building the project...'
                // Add build commands here, e.g., Maven, Gradle, npm
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                // Add testing commands here
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying the project...'
                // Add deployment commands here
            }
        }
    }
}
