pipeline {
    agent any

    environment {
        DOCKER_CREDS = credentials('dockerhub')
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
    }
}
