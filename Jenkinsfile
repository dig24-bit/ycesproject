pipeline {
    agent any
    
    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/dig24-bit/ycesproject.git'
            }
        }
        
        stage('Copy source code to Docker swarm') {
            steps {
                script {
                    try {
                        sh 'ls -la'  // Debug: list files
                        sh 'ansible-playbook playbook-to-copy-data-to-docker.yml --user=jenkins'
                    } catch (Exception e) {
                        echo "Error: ${e.toString()}"
                        // Optional: continue pipeline anyway
                        // currentBuild.result = 'UNSTABLE'
                    }
                }
            }
        }
        
        // Other stages...
    }
}
