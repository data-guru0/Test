@Library('jenkins-shared') _

pipeline {
    agent any
    environment {
        DOCKER_REPO = "dataguru97/shared-test"
    }
    stages {
        stage('Checkout') {
            steps {
                gitCheckout('https://github.com/data-guru0/Test.git', '*/main', 'github-token')
            }
        }

        stage('Build & Push Image') {
            steps {
                dockerBuildAndPush(DOCKER_REPO, 'dockerhub-token')
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                k8sDeploy('kubeconfig')
            }
        }
    }
}
