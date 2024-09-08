pipeline {
    agent any

    environment {
        IMAGE_NAME = 'jenkins_project_image'
        CONTAINER_NAME = 'jenkins_project_container'
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from the GitHub repository
                checkout scmGit(branches: [[name: 'main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/devops2846/jenkins_sample_project.git']])
            }
        }
        
        stage('Build Docker Image') {
    steps {
        script {
            sh """
            if docker image inspect ${IMAGE_NAME}:latest > /dev/null 2>&1; then
                echo "Image ${IMAGE_NAME}:latest already exists. Removing it..."
                docker rmi -f ${IMAGE_NAME}:latest
            fi

            # Build the Docker image using the Dockerfile from the repository
            docker build -t ${IMAGE_NAME}:latest .
            """
        }
    }
}


        stage('Run Docker Container') {
            steps {
                script {
                    // Stop and remove any existing container with the same name
                    sh """
                    docker stop ${CONTAINER_NAME} || true
                    docker rm ${CONTAINER_NAME} || true
                    """
                    // Run the Docker container
                    sh """
                    docker run -d --name ${CONTAINER_NAME} -p 8181:80 ${IMAGE_NAME}:latest
                    """
                }
            }
        }
    }
}
