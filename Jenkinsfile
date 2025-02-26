pipeline {
    agent any

    environment {
        KUBECONFIG = "/var/lib/jenkins/.kube/config"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'gh-pages', url: 'https://github.com/anni1526/beginner-html-site-styled.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t anni1526/beginner-html-site-styled:latest .'
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'b2c9ea3c-9835-4f33-a03a-e6ce3d5c8c1d', usernameVariable: 'DOCKERHUB_USERNAME', passwordVariable: 'DOCKERHUB_PASSWORD')]) {
                        sh 'docker login -u $DOCKERHUB_USERNAME -p $DOCKERHUB_PASSWORD'
                        sh 'docker push anni1526/beginner-html-site-styled:latest'
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    sh 'kubectl apply -f deployment.yaml || exit 1'
                    sh 'kubectl apply -f service.yaml || exit 1'
                }
            }
        }
    }

    post {
        failure {
            echo 'Pipeline failed! Check the logs for details.'
        }
        success {
            echo 'Pipeline succeeded! Deployment completed.'
        }
    }
}
