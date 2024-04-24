pipeline {
    agent any 

    environment {
        //DOCKER_IMAGE_TAG = '1'
        DOCKER_HUB_REPO = 'herasidi/django-notes-app'
    }

    stages {
        stage("Clone Code") {
            steps {
                echo "Cloning the code"
                git url: "https://github.com/heramatagne/my-containerization-deployment.git", branch: "main"
            }
        }
        stage("Build") {
            steps {
                echo "Building the image"
                sh "docker build -t ${DOCKER_HUB_REPO}:latest ."
            }
        }
        stage("Push to Docker Hub") {
            steps {
                echo "Pushing the image to Docker Hub"
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerHub') {
                        docker.image("${DOCKER_HUB_REPO}:latest").push()
                    }
                }
            }
        }
        stage("Deploy") {
            steps {
                echo "Deploying the container"
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
