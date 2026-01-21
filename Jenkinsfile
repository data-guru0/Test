pipeline {
    agent any
    environment {
        DOCKER_HUB_REPO = "dataguru97/shared-test"
        DOCKER_HUB_CREDENTIALS_ID = "dockerhub-token"
        KUBECONFIG_CREDENTIALS = "kubeconfig"
    }
    stages {
        stage('Checkout GitHub') {
            steps {
                echo 'Checking out code from GitHub...'
                checkout scmGit(
                    branches: [[name: '*/main']],
                    userRemoteConfigs: [[credentialsId: 'github-token', url: 'https://github.com/data-guru0/GITOPS-PROJECT-9.git']]
                )
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    echo 'Building Docker image...'
                    dockerImage = docker.build("${DOCKER_HUB_REPO}:latest")
                }
            }
        }

        stage('Push to DockerHub') {
            steps {
                script {
                    echo 'Pushing Docker image to DockerHub...'
                    docker.withRegistry('https://registry.hub.docker.com', "${DOCKER_HUB_CREDENTIALS_ID}") {
                        dockerImage.push('latest')
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    echo 'Applying K8s manifests...'
                    kubeconfig(credentialsId: "${KUBECONFIG_CREDENTIALS}", serverUrl: "") {
                        sh """
                        kubectl apply -f k8s/deployment.yaml
                        kubectl apply -f k8s/service.yaml
                        """
                    }
                }
            }
        }
    }
}
