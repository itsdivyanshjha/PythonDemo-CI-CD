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
                // SSH into the development server and run build and test commands
                sshagent(['ubuntu']) {
                    sh '''
                    echo "Starting Build and Test on Development Server"
                    ssh -o StrictHostKeyChecking=no ubuntu@3.111.157.57 << EOF
                        echo "SSH into Development Server"
                        if [ ! -d "~/my-python-project" ]; then
                            echo "Cloning repository"
                            git clone https://github.com/itsdivyanshjha/my-python-project.git
                        else
                            echo "Pulling latest changes"
                            cd ~/my-python-project
                            git pull origin main
                        fi
                        cd ~/my-python-project
                        echo "Setting up virtual environment"
                        python3 -m venv venv
                        source venv/bin/activate
                        echo "Installing requirements"
                        pip install -r requirements.txt
                        echo "Running tests"
                        python -m unittest discover
                    EOF
                    '''
                }
            }
        }
        stage('Deploy to Production Server') {
            steps {
                script {
                    // Only deploy if the previous stage was successful
                    if (currentBuild.result == null || currentBuild.result == 'SUCCESS') {
                        sshagent(['ubuntu']) {
                            sh '''
                            echo "Starting Deployment to Production Server"
                            ssh -o StrictHostKeyChecking=no ubuntu@13.201.84.95 << EOF
                                echo "SSH into Production Server"
                                if [ ! -d "~/my-python-project" ]; then
                                    echo "Cloning repository"
                                    git clone https://github.com/itsdivyanshjha/my-python-project.git
                                else
                                    echo "Pulling latest changes"
                                    cd ~/my-python-project
                                    git pull origin main
                                fi
                                cd ~/my-python-project
                                echo "Setting up virtual environment"
                                python3 -m venv venv
                                source venv/bin/activate
                                echo "Installing requirements"
                                pip install -r requirements.txt
                                echo "Starting application"
                                nohup python app.py &
                            EOF
                            '''
                        }
                    }
                }
            }
        }
    }
}
