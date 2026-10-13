pipeline {
    agent any

    environment {
        KUBECONFIG = '/var/lib/jenkins/.kube/config'
    }

    stages {
        stage('Check Environment') {
            steps {
                sh 'kubectl get nodes'
                sh 'kubectl get deployments'
            }
        }

        stage('Deploy Green') {
            steps {
                sh 'kubectl apply -f deployment-green.yaml'
                sh 'kubectl rollout status deployment/myapp-green --timeout=120s'
            }
        }

        stage('Switch Traffic to Green') {
            steps {
                sh """kubectl patch service myapp-service \
                -p '{"spec":{"selector":{"app":"myapp","color":"green"}}}'"""
            }
        }

        stage('Verify Deployment') {
            steps {
                sh 'kubectl get service myapp-service'
                sh 'kubectl get pods'
            }
        }
    }

    post {
        success {
            echo 'Green deployment completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check the console output.'
        }
    }
}
