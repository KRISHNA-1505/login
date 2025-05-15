pipeline {
    agent any

    environment {
        IMAGE_NAME = "krishna728/education-animation"
        TAG = "v1"
        DOCKER_USERNAME = "1234567890"
        KUBECONFIG = "/home/kisanth/.kube/config"  // Make sure Jenkins user can access this
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
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    script {
                        sh "echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin"
                        docker.image("${IMAGE_NAME}:${TAG}").push()
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    sh "kubectl --kubeconfig=${KUBECONFIG} apply -f k8s/deployment.yaml --validate=false"
                    
                }
            }
        }
    }
}
