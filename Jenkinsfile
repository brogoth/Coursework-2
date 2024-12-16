pipeline {
    agent any
    
    environment {
        DOCKER_IMAGE = "yourdockerhubusername/coursework-image:1.0"  
        DOCKER_HUB_CREDENTIALS = credentials('')  // 
        GITHUB_REPO = "https://github.com/brogoth/Coursework-2.git"  // The GitHub repository URL
    }

    stages {
        
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: "${GITHUB_REPO}"
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t ${DOCKER_IMAGE} .'
            }
        }

        stage('Run Docker Container') {
            steps {
                sh 'docker run -d -p 9090:8080 ${DOCKER_IMAGE}'
            }
        }

        stage('Test Container Launch') {
            steps {
                script {
                    // Ensure the container is running correctly by executing a command inside it
                    sh '''
                    docker exec $(docker ps -q --filter ancestor=${DOCKER_IMAGE}) echo "Container is running"
                    '''
                }
            }
        }

        stage('Push Docker Image to DockerHub') {
            steps {
                script {
                    // Log in to DockerHub and push the image
                    withCredentials([usernamePassword(credentialsId: "${DOCKER_HUB_CREDENTIALS}", usernameVariable: 'brogoth', passwordVariable: 'kiKuzz10!')]) {
                        sh '''
                        echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                        docker push ${DOCKER_IMAGE}
                        '''
                    }
                }
            }
        }

    }
}

