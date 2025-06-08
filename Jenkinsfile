pipeline {
    agent any

    environment {
        PRIVATE_KEY = "~/.ssh/CICD.pem"
        INVENTORY_MANAGER = "13.200.222.125,"
        INVENTORY_WORKER  = "13.204.77.82,"
    }

    stages {
        stage('Ansible - Copy Data') {
            steps {
                sh """
                ansible-playbook -i ${INVENTORY_MANAGER} source-code/Docker/ansible/playbook-to-copy-data-to-docker.yml -u ubuntu --private-key ${PRIVATE_KEY}
                ansible-playbook -i ${INVENTORY_WORKER} source-code/Docker/ansible/playbook-to-copy-data-to-docker.yml -u ubuntu --private-key ${PRIVATE_KEY}
                """
            }
        }

        stage('Ansible - Push Images') {
            steps {
                sh """
                ansible-playbook -i ${INVENTORY_MANAGER} source-code/Docker/ansible/playbook-to-push.yml -u ubuntu --private-key ${PRIVATE_KEY}
                ansible-playbook -i ${INVENTORY_WORKER} source-code/Docker/ansible/playbook-to-push.yml -u ubuntu --private-key ${PRIVATE_KEY}
                """
            }
        }

        stage('Ansible - Deploy to Swarm') {
            steps {
                sh """
                ansible-playbook -i ${INVENTORY_MANAGER} source-code/Docker/ansible/playbook-for-deployment.yml -u ubuntu --private-key ${PRIVATE_KEY}
                """
            }
        }
    }
}



