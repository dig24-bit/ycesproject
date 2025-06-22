pipeline {
    agent any

    environment {
        IMAGE_NAME = "ycesproject"
        DOCKER_REGISTRY = "your-dockerhub-username" // Replace with your actual DockerHub username
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
                    sh 'docker build -t $IMAGE_NAME .'
                }
            }
        }

        stage('Tag and Push Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    script {
                        sh 'docker tag $IMAGE_NAME $DOCKER_USER/$IMAGE_NAME:latest'
                        sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                        sh 'docker push $DOCKER_USER/$IMAGE_NAME:latest'
                    }
                }
            }
        }

        stage('Deploy to Docker Swarm') {
            steps {
                script {
                    sh '''
                    docker pull $DOCKER_USER/$IMAGE_NAME:latest
                    docker service update --image $DOCKER_USER/$IMAGE_NAME:latest ycesproject_service || \
                    docker service create --name ycesproject_service -p 80:80 $DOCKER_USER/$IMAGE_NAME:latest
                    '''
                }
            }
        }
    }
}
