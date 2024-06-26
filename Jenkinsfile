pipeline {
    agent any

    environment {
        DEPLOY_DIR = '/home/ec2-user/code deployment'
        SSH_KEY = credentials('ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDlfT+OmHKTdhLvOEP1KBJvb50aDVp6ipYRRPKSLyMZQ+a9I6a+9Kv7KxfIjkl9iuQX1ibfqxGONIUR6y1v4Sc20d6VcQeTqwagOECqIx/R7qxsuOAN6KppV2u9bz/dN8xDyfo9SaT3mJA6IXtTa/b9AGHtAAAs94di5qDh3576WFPLzQKRlzDBWUaqeVHzNpyrV+5VqX8yeSwiXMLeXcqfYWNFQ+BtYukmiWj3ZNdVLNRcQYLW8Qt7RwLEnaUhGGi4cyUDqYdr+73ZJoSR7FrysGRj32JF5vq8jtYtQnqvCWKbVYr04qT2uuoAWH4ZRTzeosxL5/RZf26KUclp7E/l ec2-user@ip-10-0-1-217.ap-south-1.compute.internal') // Ensure you have added your SSH key in Jenkins credentials
        VM1_IP = '13.233.132.230'
        VM2_IP = '65.0.107.249'
        USER = 'ec2-user'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git url: 'https://github.com/itsdivyanshjha/PythonDemo-CI-CD.git', branch: 'main'
            }
        }

        stage('Build') {
            steps {
                script {
                    def image = docker.build("flask-demo:${env.BUILD_ID}")
                    image.inside {
                        sh 'pip install -r requirements.txt'
                    }
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    def image = docker.build("flask-demo:${env.BUILD_ID}")
                    image.inside {
                        sh 'python -m unittest discover -s tests'
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                sshagent(['my-ssh-key']) {
                    sh '''
                    scp -r * ${USER}@${VM2_IP}:${DEPLOY_DIR}
                    ssh ${USER}@${VM2_IP} "docker build -t flask-demo ${DEPLOY_DIR} && docker run -d -p 5000:5000 flask-demo"
                    '''
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully.'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}