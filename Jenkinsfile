pipeline {
       agent { label 'docker-agent-1' }

    environment {
        IMAGE_NAME = "ycesproject"
        DOCKER_USER = "digvijay554" // <-- Replace with your actual DockerHub username
    }

    stages {

        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/dig24-bit/ycesproject.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${DOCKER_USER}/${IMAGE_NAME}:9.0.0", ".")
                }
            }
        }

        stage('Tag and Push Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    script {
                        sh "echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin"
                        sh "docker push $DOCKER_USER/$IMAGE_NAME:9.0.0"
                    }
                }
            }
        }

        stage('Deploy to Docker Swarm') {
            steps {
                script {
                    sh """
                        docker pull $DOCKER_USER/$IMAGE_NAME:9.0.4
                        docker service update --image $DOCKER_USER/$IMAGE_NAME:9.0.4 ycesproject_web || \
                        docker service create --name ycesproject_web -p 8085:80 $DOCKER_USER/$IMAGE_NAME:9.0.4
                    """
                }
            }
        }

    }
}

