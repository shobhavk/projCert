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
        sh 'docker build -t sshobha379/phpapp:latest .'
    }
}


stage('Push Docker Image') {
    steps {
        withCredentials([usernamePassword(credentialsId: 'docker_username', usernameVariable: 'sshobha379', passwordVariable: 'password')]) {
            sh '''
                echo "password" | docker login -u "sshobha379" --password-stdin
                docker push sshobha379/phpapp:latest
            '''
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
