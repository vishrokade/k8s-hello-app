pipeline {
    agent any

    environment {
        KUBECONFIG_CRED_ID = 'kubeconfig'
    }

    stages {
        stage('Checkout') {
            steps {
                git credentialsId: 'github-creds', url: 'https://github.com/vishrokade/k8s-hello-app.git'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                withCredentials([file(credentialsId: env.KUBECONFIG_CRED_ID, variable: 'KUBECONFIG')]) {
                    sh '''
                        export KUBECONFIG=$KUBECONFIG
                        kubectl apply -f deployment.yaml
                        kubectl apply -f service.yaml
                    '''
                }
            }
        }

        stage('Test Deployment') {
            steps {
                sh '''
                    kubectl get pods
                    kubectl get svc hello-service
                '''
            }
        }
    }
}
