pipeline {
    agent any

    environment {
        DEV_SERVER = 'ubuntu@13.127.73.63'
        PROD_SERVER = 'ubuntu@3.111.197.175'
        SSH_KEY_PATH = '/home/ubuntu/.ssh/id_rsa'
        REPO_URL = 'https://github.com/itsdivyanshjha/my-python-project.git'
        PROJECT_DIR = '~/my-python-project'
    }

    stages {
        stage('Clone Repository') {
            steps {
                // Clone the repository from GitHub
                git url: env.REPO_URL, branch: 'main'
            }
        }

        stage('Build and Test on Development Server') {
            steps {
                script {
                    sh """
                    ssh -i ${env.SSH_KEY_PATH} -o StrictHostKeyChecking=no ${env.DEV_SERVER} << EOF
                        if [ ! -d "${env.PROJECT_DIR}" ]; then
                            git clone ${env.REPO_URL} ${env.PROJECT_DIR}
                        fi
                        cd ${env.PROJECT_DIR}
                        git pull origin main
                        python3 -m venv venv
                        pip install -r requirements.txt
                        python -m unittest discover || exit 1
                    EOF
                    """
                }
            }
        }

        stage('Deploy to Production Server') {
            when {
                expression {
                    currentBuild.result == null || currentBuild.result == 'SUCCESS'
                }
            }
            steps {
                script {
                    sh """
                    ssh -i ${env.SSH_KEY_PATH} -o StrictHostKeyChecking=no ${env.PROD_SERVER} << EOF
                        if [ ! -d "${env.PROJECT_DIR}" ]; then
                            git clone ${env.REPO_URL} ${env.PROJECT_DIR}
                        fi
                        cd ${env.PROJECT_DIR}
                        git pull origin main
                        python3 -m venv venv
                        pip install -r requirements.txt
                        nohup python app.py &
                    EOF
                    """
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
