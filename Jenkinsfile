pipeline {
    agent any
    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/brogoth/Coursework-2.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t coursework-image:1.0 .'
            }
        }
        stage('Run Docker Container') {
    steps {
        sh 'docker run -d -p 9090:8080 coursework-image:1.0'
          }
        }
    }
}
