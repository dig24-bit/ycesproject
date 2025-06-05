pipeline {
    agent any

    stages {
        stage('Clone GitHub Repository') {
            steps {
                // Cloning the main branch from GitHub
                git branch: 'main', url: 'https://github.com/dig24-bit/ycesproject.git'
            }
        }

        stage('Copy source code to Docker Swarm') {
            steps {
                //  "ycesproject/" /var/lib/jenkins/workspace/<JOB_NAME>/source-code/Docker/ansible/playbook-to-copy-data-to-docker.yml

                sh '''
                ansible-playbook -i 13.201.102.215, source-code/Docker/ansible/playbook-to-copy-data-to-docker.yml -u ubuntu --private-key ~/.ssh/CICD.pem
                '''
            }
        }

        stage('Build & Push the new Image to Dockerhub') {
            steps {
                //  "ycesproject/"/var/lib/jenkins/workspace/<JOB_NAME>/source-code/Docker/ansible/playbook-to-copy-data-to-docker.yml

                sh '''
                ansible-playbook -i 13.200.222.125, source-code/Docker/ansible/playbook-to-push.yml -u ubuntu --private-key ~/.ssh/CICD.pem
                '''
            }
        }

        stage('Deploying new Service in Docker Swarm') {
            steps {
                // "ycesproject/"/var/lib/jenkins/workspace/<JOB_NAME>/source-code/Docker/ansible/playbook-to-copy-data-to-docker.yml 
                sh '''
                ansible-playbook -i 13.200.222.125, source-code/Docker/ansible/playbook-for-deployment.yml -u ubuntu --private-key ~/.CICD.pem
                '''
            }
        }
    }
}


