pipeline {
    agent any
    stages {
        stage('docker cleaning') {
            steps {
                echo "cleaning up docker"
                script {
                    //Stop and remove all running containers
                    sh 'sudo docker stop $(sudo docker ps -a -q) || true'
                    sh 'sudo docker rm $(sudo docker ps -a -q) || true'
                    //Remove all docker images
                    sh 'sudo docker rmi $(sudo docker images -a -q) || true'
                    //Cleaning up any other docker resources
                    sh 'sudo docker system prune -f || true'
                }
                echo 'Cleaning up completed'
            }
        }
        stage('check for backend package.json') {
            steps {
                script {
                    echo 'checking if package.json exists in the directory'
                    dir('api'){
                        sh 'ls -l package.json'
                    }
                }
                
            }
        }
        stage('Build and Push Backend docker images') {
            steps {
                echo "build docker images and push to docker hub"
                dir('api') {
                    script {
                        docker.withRegistry('https://hub.docker.com', 'ravisaketi08') {
                        // Docker build and push commands
                        docker.build('backend-app:latest').push()
                        }
                    }
                }
            }
        }
        stage('check for frontend package.json') {
            steps {
                script {
                    echo 'checking if package.json exists in the directory'
                    dir('webapp'){
                        sh 'ls -l package.json'
                    }
                }
                
            }
        }
        stage('Build and Push frontend docker images') {
            steps {
                echo "build docker images and push to docker hub"
                dir('webapp') {
                    script {
                        docker.withRegistry('https://hub.docker.com', 'ravisaketi08') {
                        // Docker build and push commands
                        docker.build('frontend-app:latest').push()
                        }
                    }
                }
            }
        }
    }
}
