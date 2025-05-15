pipeline {
    agent any
    
    environment {
        DOCKER_IMAGE = "yourdockerhubusername/my-app:${env.BUILD_NUMBER}"
        KUBECONFIG = 'kubeconfig' // this is Jenkins credential ID for kubeconfig file
    }
    
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/yourusername/my-app.git'
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build(DOCKER_IMAGE)
                }
            }
        }
        
        stage('Push Docker Image') {
            steps {
                withDockerRegistry(credentialsId: 'dockerhub-credentials', url: '') {
                    script {
                        docker.image(DOCKER_IMAGE).push()
                    }
                }
            }
        }
        
        stage('Deploy to Kubernetes') {
            steps {
                withCredentials([file(credentialsId: KUBECONFIG, variable: 'KUBECONFIG_PATH')]) {
                    sh '''
                    export KUBECONFIG=$KUBECONFIG_PATH
                    kubectl apply -f k8s-deployment.yaml
                    '''
                }
            }
        }
    }
}
