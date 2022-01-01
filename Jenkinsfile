pipeline {
    agent any
    stages {
        stage('Copy secrets file') {
        stage('Helm Dry Run') {
            agent {
                docker {
                    image 'dtzar/helm-kubectl:latest'
                    args '-v /var/lib/jenkins/config:/var/lib/jenkins/config'
                }
            }
            steps {
                    echo 'Deploying using helm...'
                    sh 'export KUBECONFIG=/var/lib/jenkins/config && helm upgrade --install jenkins ./jenkins/ -i -n jenkins -f ./jenkins/values.yaml --dry-run'
            }
        }
        stage('Deploy to k3s') {
            when {
                expression {
                    env.BRANCH_NAME == 'master'
                }
            }
            agent {
                docker {
                    image 'dtzar/helm-kubectl:latest'
                    args '-v /var/lib/jenkins/config:/var/lib/jenkins/config'
                }
            }
            steps {
                    echo 'Deploying using helm...'
                    sh 'export KUBECONFIG=/var/lib/jenkins/config && helm upgrade --install jenkins ./jenkins/ -i -n jenkins -f ./jenkins/values.yaml'
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }
}
