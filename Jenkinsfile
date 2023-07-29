pipeline {
    agent any

    stages {
        stage("Clone Code") {
            steps {
                echo "Cloning the code"
                git url: "https://github.com/Isha-Ipsita/django-notes-app.git", branch: "main"
            }
        }

        stage("Build") {
            steps {
                echo "Building the code"
                sh "docker build -t my-note-app ."
            }
        }

        stage("Push to Docker Hub") {
            steps {
                echo "Pushing the image to Docker Hub"
                withCredentials([usernamePassword(credentialsId: "DockerHubID", passwordVariable: "dockerHubPass", usernameVariable: "dockerHubUser")]) {
                    sh "docker tag my-note-app ${dockerHubUser}/my-note-app:latest"
                    sh 'echo "${dockerHubPass}" | docker login -u "${dockerHubUser}" --password-stdin'
                    sh "docker push ${dockerHubUser}/my-note-app:latest"
                }
            }
        }

        stage("Deploy") {
            steps {
                echo "Deploying the container on EC2"
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
