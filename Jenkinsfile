pipeline {
    agent any

    environment {
        IMAGE_NAME = "applebiteco/phpapp"
        DEV_HOST = "dev.example.com"
        PROD_HOST = "prod.example.com"
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'master', url: 'https://github.com/shobhavk/projCert.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("sshobha379/phpapp:latest")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('', 'docker_username') {
                        dockerImage.push()
                    }
                }
            }
        }
        
        stage('Deploy to Dev') {
            steps {
                script {
                    sh "ansible-playbook -i ansible/inventory ansible/dev.yml"
                }
            }
        }

        stage('Run Tests') {
            steps {
                // Add your test scripts here
                echo "Running tests on Dev server..."
            }
        }

        stage('Deploy to Prod') {
            when {
                expression { return currentBuild.result == null || currentBuild.result == 'SUCCESS' }
            }
            steps {
                script {
                    sh "ansible-playbook -i ansible/inventory ansible/prod.yml"
                }
            }
        }
    }
}
