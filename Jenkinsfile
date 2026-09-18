pipeline {
    agent {
        node {
            label ''  // Or use 'agent any' if you do not need to target a specific node
        }
    }

    stages {
        stage('Build') {
            steps {
                script {
                    git branch: 'main', url: 'https://github.com/ganeshkumar14/spring-petclinic-microservices.git'
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    sh 'echo "Testing started"'
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    sh 'echo "Deploying code"'
                }
            }
        }
    }
}
