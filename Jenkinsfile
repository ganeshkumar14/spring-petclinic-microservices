pipeline {
    agent {
        // This spins up a temporary sibling container on your GCP host to run the build commands
        docker {
            image 'maven:3.9-eclipse-temurin-17'
            // Mounts the host's Docker socket so the container can build new Docker images
            args '-v /var/run/docker.sock:/var/run/docker.sock'
        }
    }

    environment {
        // Using the GitHub credential ID we discussed earlier to securely authenticate
        GITHUB_CREDENTIALS_ID = 'github-token' 
    }

    stages {
        stage('Checkout') {
            steps {
                // Securely checks out the repository using your authenticated GitHub token
                git branch: 'main', 
                    credentialsId: "${env.GITHUB_CREDENTIALS_ID}", 
                    url: 'https://github.com/ganeshkumar14/spring-petclinic-microservices.git'
            }
        }

        stage('Build & Package Images') {
            steps {
                echo 'Compiling Spring Boot apps and packaging them into local Docker images...'
                // Automatically uses the project's buildDocker profile
                sh './mvnw clean install -P buildDocker'
            }
        }

        stage('Test Verification') {
            steps {
                echo 'Running unit and integration tests...'
                sh './mvnw test'
            }
        }

        stage('Verify Local Images') {
            steps {
                echo 'Listing the newly created microservice images on the GCP VM host:'
                // This talks directly to the host's Docker daemon
                sh 'docker images | grep spring-petclinic'
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully! Microservice images are ready on the GCP host.'
        }
        failure {
            echo 'Pipeline failed. Please review the build logs above for syntax or compilation errors.'
        }
    }
}
