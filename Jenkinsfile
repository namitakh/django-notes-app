pipeline {
    agent any
    stages {
        stage("Cloning the Code") {
            steps {
                echo "Cloning the code"
                git url: "https://github.com/namitakh/django-notes-app.git", branch: "main"
            }
        }
        stage("Build") {
            steps {
                echo "Building the Docker image"
                sh "docker build -t django-app ."
            }
        }
        stage("Push to Docker Hub") {
            steps {
                echo "Pushing image to Docker Hub"
                withCredentials([usernamePassword(credentialsId: "dockerhub", usernameVariable: "DOCKER_USER", passwordVariable: "DOCKER_PASS")]) {
                    sh "docker login -u $DOCKER_USER -p $DOCKER_PASS"
                    sh "docker tag django-app $DOCKER_USER/django-app:latest"
                    sh "docker push $DOCKER_USER/django-app:latest"
                }
            }
        }
        stage("Deploy") {
            steps {
                echo "Deploying the container"
                sh """
                    docker-compose down
                    docker-compose up -d
                """
            }
        }
    }
}
