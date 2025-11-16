pipeline {
    agent any

    environment {
        DOCKERHUB_USER = "meghana1724"
        IMAGE_NAME = "productservice"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/Meghana2417/ProductService.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                python3 -m venv myenv
                . myenv/bin/activate
                pip install --upgrade pip
                pip install -r requirements.txt
                '''
            }
        }

        stage('Run Migrations Test') {
            steps {
                sh "python manage.py test || true"
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${DOCKERHUB_USER}/${IMAGE_NAME}:latest ."
            }
        }

        stage('Login to DockerHub') {
            steps {
                withCredentials([string(credentialsId: 'dockerhub-pass', variable: 'PASS')]) {
                    sh "echo $PASS | docker login -u ${DOCKERHUB_USER} --password-stdin"
                }
            }
        }

        stage('Push Image') {
            steps {
                sh "docker push ${DOCKERHUB_USER}/${IMAGE_NAME}:latest"
            }
        }

        stage('Deploy on EC2') {
            steps {
                sshagent(['ec2-ssh']) {
                    sh '''
                    ssh -o StrictHostKeyChecking=no ubuntu@YOUR_EC2_IP "
                        docker pull meghana1724/productservice:latest &&
                        docker stop django || true &&
                        docker rm django || true &&
                        docker run -d --name django -p 8003:8003 meghana1724/productservice:latest
                    "
                    '''
                }
            }
        }
    }
}
