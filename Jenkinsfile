pipeline {
    agent any

    stages {
        stage('Ansible - Copy Data') {
            steps {
                sh '''
                ansible-playbook -i 13.204.77.82, source-code/Docker/ansible/playbook-to-copy-data-to-docker.yml -u ubuntu --private-key ~/.ssh/CICD.pem
                '''
            }
        }

        stage('Ansible - Push') {
            steps {
                sh '''
                ansible-playbook -i 13.204.77.82, source-code/Docker/ansible/playbook-to-push.yml -u ubuntu --private-key ~/.ssh/CICD.pem
                '''
            }
        }

        stage('Ansible - Deploy') {
            steps {
                sh '''
                ansible-playbook -i 13.204.77.82, source-code/Docker/ansible/playbook-for-deployment.yml -u ubuntu --private-key ~/.ssh/CICD.pem
                '''
            }
        }
    }
}


