pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build and Push Docker Images') {
            steps {
                script {
                    def dockerImageName = "anishaaaaa/myphp:latest"

                    // Build the Apache Docker image
                    bat "docker build -t $dockerImageName -f Dockerfile ."

                    // Push the Docker image to a container registry (e.g., Docker Hub)
                    bat 'docker login -u anishaaaaa -p anisha12345'
                    bat "docker push $dockerImageName"
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    def mysqlPodName = 'mysql-pod'
                    def phpAdminPodName = 'phpadmin-pod'

                    // Apply the MySQL and PHPMyAdmin YAML files to the Kubernetes cluster
                    bat "kubectl apply -f mysql-pod.yaml"
                    bat "kubectl apply -f phpadmin.yaml"

                    // Wait for the MySQL and PHPMyAdmin pods to be ready (you may need to adjust the timeouts)
                    bat "kubectl wait --for=condition=ready pod/$mysqlPodName --timeout=300s"
                    bat "kubectl wait --for=condition=ready pod/$phpAdminPodName --timeout=300s"
                }
            }
        }
    }

    post {
        failure {
            echo 'The pipeline has failed!'
        }
        success {
            echo 'The pipeline has succeeded!'
        }
    }
}
