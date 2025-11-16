pipeline {
    agent any
    environment {
        DOCKER_USERNAME = credentials('dockerhub').username
        DOCKER_PASSWORD = credentials('dockerhub').password
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
                sh 'echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USERNAME" --password-stdin'
            }
        }

        stage('Docker Push') {
            steps {
                sh "docker push meghana1724/productservice"
            }
        }
    }
}
