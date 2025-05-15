pipeline {
    agent any

    environment {
        IMAGE_NAME = "krishna728/education-animation"
        TAG = "v1"
        DOCKER_USERNAME = "krishna728"
        DOCKER_PASSWORD = "1234567890"  // Not secure - avoid using in production
        KUBECONFIG = "/home/kisanth/.kube/config"  // Set the correct kubeconfig path
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/KRISHNA-1505/login.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${IMAGE_NAME}:${TAG}")
                }
            }
        }

                stage('Push to Docker Hub') {
                    steps {
                        withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials1', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                            sh '''
                                echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin
                                docker push krishna728/education-animation:v1
                            '''
                        }
                    }
        }


        stage('Deploy to Kubernetes') {
            steps {
                script {
                    // Apply Kubernetes deployment and service using kubectl
                    sh 'kubectl apply -f k8s/deployment.yaml --validate=false'
                    sh 'kubectl apply -f k8s/service.yaml --validate=false'
                }
            }
        }
    }
}
