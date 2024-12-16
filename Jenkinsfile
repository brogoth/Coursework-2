pipeline {
    agent any
    
    environment {
        DOCKER_IMAGE = "yourdockerhubusername/coursework-image:1.0"  // Replace with my DockerHub username and image name if it ever uninstalls
        DOCKER_HUB_CREDENTIALS = credentials('dockerhub-credentials-id')  // Replace with my DockerHub credentials ID if i can ever access
        GITHUB_REPO = "https://github.com/brogoth/Coursework-2.git"  // My GitHub repository URL
        K8S_NAMESPACE = "default"  // Kubernetes namespace to deploy to 
        DEPLOYMENT_NAME = "your-deployment-name"  // The Kubernetes deployment name
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
                    withCredentials([usernamePassword(credentialsId: "${DOCKER_HUB_CREDENTIALS}", usernameVariable: 'brogoth', passwordVariable: 'kiKuzz10!')]) {
                        sh '''
                        echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                        docker push ${DOCKER_IMAGE}
                        '''
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    // Update the Kubernetes deployment with the new image using rolling update
                    sh '''
                    kubectl set image deployment/${DEPLOYMENT_NAME} ${DEPLOYMENT_NAME}=${DOCKER_IMAGE} --namespace=${K8S_NAMESPACE}
                    kubectl rollout status deployment/${DEPLOYMENT_NAME} --namespace=${K8S_NAMESPACE}
                    '''
                }
            }
        }
    }
}
