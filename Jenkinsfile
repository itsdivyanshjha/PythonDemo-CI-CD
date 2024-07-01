pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                // Clone the repository from GitHub
                git url: 'https://github.com/itsdivyanshjha/my-python-project.git', branch: 'main'
            }
        }
        stage('Build and Test on Development Server') {
            steps {
                sh '''
                ssh -i /var/lib/jenkins/.ssh/id_rsa -o StrictHostKeyChecking=no ubuntu@13.127.73.63 << EOF
                    cd ~/my-python-project
                    git pull origin main
                    python3 -m venv venv
                    source venv/bin/activate
                    pip install -r requirements.txt
                    python -m unittest discover
                EOF
                '''
            }
        }
        stage('Deploy to Production Server') {
            steps {
                sh '''
                ssh -i /var/lib/jenkins/.ssh/id_rsa -o StrictHostKeyChecking=no ubuntu@3.111.197.175 << EOF
                    cd ~/my-python-project
                    git pull origin main
                    python3 -m venv venv
                    source venv/bin/activate
                    pip install -r requirements.txt
                    nohup python app.py &
                EOF
                '''
            }
        }
    }
}
