pipeline {
    agent any

    stages {
        stage('Clone GitHub Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/dig24-bit/ycesproject.git'
            }
        }

        stage('Copy source code to Docker Swarm') {
            steps {
                sh  sh '''
ansible-playbook -i 3.108.55.248, source-code/Docker/ansible/playbook-to-copy-data-to-docker.yml -u ubuntu --private-key ~/.ssh/your-key.pem
'''
'
            }
        }

        stage('Build & Push the new Image to Dockerhub') {
            steps {
                sh 'ansible-playbook -i "3.108.55.248," ansible/playbook-to-push.yml -u ubuntu'
            }
        }

        stage('Deploying new Service in Docker Swarm') {
            steps {
                sh'ansible-playbook-i"3.108.55.248,"ansible/playbook-for-deployment.yml-u ubuntu'
            }
        }
    }
}
