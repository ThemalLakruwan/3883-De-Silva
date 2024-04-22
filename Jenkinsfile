pipeline {
    agent any 
    
    stages { 
        stage('SCM Checkout') {
            steps {
                retry(3) {
                    git branch: 'main', url: 'https://github.com/ThemalLakruwan/test-node'
                }
            }
        }
        stage('Build Docker Image') {
            steps {  
                bat 'docker build -t themaldesilva/nodeapp-cuban:%BUILD_NUMBER% .'
            }
        }
        stage('Login to Docker Hub') {
            steps {
                withCredentials([string(credentialsId: 'themal-docker', variable: 'themaldocker')]) {
                    script {
                        bat "docker login -u themaldesilva -p ${themaldocker}"
                    }
                }
            }
        }
        stage('Push Image') {
            steps {
                bat 'docker push themaldesilva/nodeapp-cuban:%BUILD_NUMBER%'
            }
        }
    }
    post {
        always {
            bat 'docker logout'
        }
    }
}
