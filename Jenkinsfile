pipeline {
    agent any

    environment {
        DOCKER_CREDS = credentials('docker-hub')
    }

    stages {
        stage('Clone Repo') {
            steps {
                git branch: 'main', url: 'https://github.com/Meghana2417/ProductService.git'
            }
        }

        stage('Docker Build') {
            steps {
                sh "docker build -t meghana1724/productservice ."
            }
        }

        stage('Docker Login') {
            steps {
                sh 'echo "$DOCKER_CREDS_PSW" | docker login -u "$DOCKER_CREDS_USR" --password-stdin'
            }
        }

        stage('Docker Push') {
            steps {
                sh "docker push meghana1724/productservice"
            }
        }

        stage('Deploy') {
        steps {
            sh '''
            docker stop productservice || true
            docker rm productservice || true

            docker pull meghana1724/productservice:latest

            docker run -d --name productservice -p 8003:8003 meghana1724/productservice:latest
            '''
            }
        }
    }
}
