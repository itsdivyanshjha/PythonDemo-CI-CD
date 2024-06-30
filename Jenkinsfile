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
                    ssh -o StrictHostKeyChecking=no ubuntu@3.111.157.57 << EOF
                        if [ ! -d "~/my-python-project" ]; then
                            git clone https://github.com/itsdivyanshjha/my-python-project.git
                        else
                            cd ~/my-python-project
                            git pull origin main
                        fi
                        cd ~/my-python-project
                        python3 -m venv venv
                        source venv/bin/activate
                        pip install -r requirements.txt
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
                            ssh -o StrictHostKeyChecking=no ubuntu@13.201.84.95 << EOF
                                if [ ! -d "~/my-python-project" ]; then
                                    git clone https://github.com/itsdivyanshjha/my-python-project.git
                                else
                                    cd ~/my-python-project
                                    git pull origin main
                                fi
                                cd ~/my-python-project
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
        }
    }
}
