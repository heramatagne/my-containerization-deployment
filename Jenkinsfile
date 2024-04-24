pipeline {
    agent any 

    environment {
        DOCKER_IMAGE_TAG = 'django'
        // KUBECONFIG = credentials('your-kubeconfig-credential-id')
        DOCKER_HUB_REPO = 'herasidi/centos_webapp' // Define your Docker Hub repository name      
    }

    stages{
        stage("Clone Code"){
            steps {
                echo "Cloning the code"
                git url:"https://github.com/heramatagne/my-containerization-deployment.git", branch: "main"
            }
        }
        stage("Build"){
            steps {
                echo "Building the image"
                sh "docker build -t my-note-app ."
            }
        }
        stage("Push to Docker Hub"){
            steps {
                echo "Pushing the image to docker hub"
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub') {
                        docker.image("${DOCKER_HUB_REPO}:${DOCKER_IMAGE_TAG}").push()
                }
            }
        }
        stage("Deploy"){
            steps {
                echo "Deploying the container"
                sh "docker-compose down && docker-compose up -d"
                
            }
        }
    }
}
