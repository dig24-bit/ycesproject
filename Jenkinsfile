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
                withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'docker user', passwordVariable: 'DOCKER_PASS')]) {
                    script {
                        sh 'docker tag ycesproject digvijay554/ycesproject:latest'
                        sh 'echo $DOCKER_PASS | docker login -u digvijay554 --password-stdin'
                        sh 'docker push digvijay554/ycesproject:latest'
                    }
                }
            }
        }

stage('Deploy to Docker Swarm') {
    steps {
        script {
            sh '''
                docker pull digvijay554/ycesproject:latest

                /usr/bin/docker service update --image digvijay554/ycesproject:latest ycesproject_web || \
                /usr/bin/docker service create --name ycesproject_web -p 80:80 digvijay554/ycesproject:latest
            '''
        }
    }
}
