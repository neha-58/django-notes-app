pipeline {
    agent any
    
    stages {
        stage("code"){
            steps {
                echo "Cloning the code"
                git url:"https://github.com/neha-58/django-notes-app.git", branch:"main"
            }
        }
        stage("build"){
            steps {
                echo "Building the image"
                sh "docker build -t my-note-app ."
            }
        }
        stage("Push to docker hub"){
            steps {
                echo "Pushing the image to Docker Hub"
                withCredentials([usernamePassword(credentialsId:"DockerHub",passwordVariable:"DockerHubPass",usernameVariable:"DockerHubUser")]){
                sh "docker tag my-note-app ${env.DockerHubUser}/my-note-app:latest"
                sh "docker login -u ${env.DockerHubUser} -p ${env.DockerHubPass}"
                sh "docker push ${env.DockerHubUser}/my-note-app:latest"
                }
            }
        }
        stage("deploy"){
            steps {
                echo "Deploying the container"
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
