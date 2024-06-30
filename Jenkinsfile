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
                sshagent(['ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQC4sis7KgGGIP/1Bfl6zVtRu9EX8n71hHW7LR0yZL6TEfJmcYRCQlQeQwZHZzlW8b1E8qS9c0KGEYrRB+NeH9rsnHOVmQ6Jeoa2OhzVc6EahNso0W5ZaeLwm7CBWue+Pg9pVNi951L0CuebTmcaHKmtQep68xsWCWCZUI+xRuka/5VbWhuwWqGzl/w0FC87Sw1AN1r8AZCVlm25sy6MKxjY9d3k4HaztrIUR36g1xuhymzUnx5E/s89NEmvnUz0L9auM7CZH2MDKwqD/Mf584nho9L+8VoxBD8tcAtpTfnAu3kltGJr20mg2MugVAGhXKjjRZw8sgPfYhO5gLE1VmHdwynI5fIy5k+HMDKeB+DzGc02cKoVKKomY9fbYvBQSJTLdzfAS55fceIJHcyLq4aX8cuDcawC1lplf3StmBISnTpXgkT+N8DN57+882kMG5z27aHuOrZIbgLOtqZHw4fi2J491QlePSI+jEWdKO0eIB8cZVTSz0fHPrssQ0QUiz5VQXpft9mEQJ1TlcmYZkFNPVdVJxNB5uWIm7RUf5GeRNiJrV17ycl82oXwXjOFy+vsQIu7iJrDQ+f7nkvhAElkYeZN1TDZY+nn82m76VZRuE2CJOEFA6zbO2fYkcvsJklqH/ckLQHL6mrNVjoeova2p/FhtUr6WAf8ClFoqOmp7w== jenkins@server
']) {
                    sh '''
                    ssh -o StrictHostKeyChecking=no ubuntu@3.111.157.57 << EOF
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
        }
        stage('Deploy to Production Server') {
            steps {
                script {
                    // Only deploy if the previous stage was successful
                    if (currentBuild.result == null || currentBuild.result == 'SUCCESS') {
                        sshagent(['ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQC4sis7KgGGIP/1Bfl6zVtRu9EX8n71hHW7LR0yZL6TEfJmcYRCQlQeQwZHZzlW8b1E8qS9c0KGEYrRB+NeH9rsnHOVmQ6Jeoa2OhzVc6EahNso0W5ZaeLwm7CBWue+Pg9pVNi951L0CuebTmcaHKmtQep68xsWCWCZUI+xRuka/5VbWhuwWqGzl/w0FC87Sw1AN1r8AZCVlm25sy6MKxjY9d3k4HaztrIUR36g1xuhymzUnx5E/s89NEmvnUz0L9auM7CZH2MDKwqD/Mf584nho9L+8VoxBD8tcAtpTfnAu3kltGJr20mg2MugVAGhXKjjRZw8sgPfYhO5gLE1VmHdwynI5fIy5k+HMDKeB+DzGc02cKoVKKomY9fbYvBQSJTLdzfAS55fceIJHcyLq4aX8cuDcawC1lplf3StmBISnTpXgkT+N8DN57+882kMG5z27aHuOrZIbgLOtqZHw4fi2J491QlePSI+jEWdKO0eIB8cZVTSz0fHPrssQ0QUiz5VQXpft9mEQJ1TlcmYZkFNPVdVJxNB5uWIm7RUf5GeRNiJrV17ycl82oXwXjOFy+vsQIu7iJrDQ+f7nkvhAElkYeZN1TDZY+nn82m76VZRuE2CJOEFA6zbO2fYkcvsJklqH/ckLQHL6mrNVjoeova2p/FhtUr6WAf8ClFoqOmp7w== jenkins@server
']) {
                            sh '''
                            ssh -o StrictHostKeyChecking=no ubuntu@13.201.84.95 << EOF
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
        }
    }
}
