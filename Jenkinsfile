pipeline {
    agent {
        docker {
            image 'eclipse-temurin:17-jdk-jammy'
            // Mounts host socket/binary AND enforces Docker BuildKit engine compilation mode
            args '-v /var/run/docker.sock:/var/run/docker.sock -v /usr/bin/docker:/usr/bin/docker --group-add 1001 -e DOCKER_TLS_VERIFY="" -e DOCKER_CERT_PATH="" -e DOCKER_BUILDKIT=1 -e COMPOSE_BUILDKIT=1'
        }
    }

    environment {
        GITHUB_CREDENTIALS_ID = 'github-token' 
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', 
                    credentialsId: "${env.GITHUB_CREDENTIALS_ID}", 
                    url: 'https://github.com/ganeshkumar14/spring-petclinic-microservices.git'
            }
        }

        stage('Build & Package Images') {
            steps {
                echo 'Compiling Spring Boot apps and packaging them into local Docker images via BuildKit...'
                sh 'chmod +x mvnw'
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
