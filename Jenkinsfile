pipeline {
    agent any
    stages {
        stage("Clone code") {
            steps {
                echo "cloning the code"
                git url: "https://github.com/rajeshyd/react-django-sqlite-project.git", branch: "main"
            }
        }
        stage("use dir and Build Backend") {
            steps {
                dir("backend") {
                    echo "Building the image backend"
                    sh  "docker build -t django-app ."
                    }
                }
            }
        stage("use dir and Build Frontend") {
            steps {
                dir("frontend") {
                    echo "Building the image of frontend"
                    sh  "docker build -t react-app ."
                    }
                }
            }
        stage("Push to docker hub") {
            steps {
                echo "push the image to docker hub"
                withCredentials([usernamePassword(credentialsId:"dockerHub", passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                    sh "docker tag django-app ${env.dockerHubUser}/django-app:latest"
                    sh "docker tag react-app  ${env.dockerHubUser}/react-app:latest"
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                    sh "docker push ${env.dockerHubUser}/django-app:latest"
                    sh "docker push ${env.dockerHubUser}/react-app:latest"
                }
            }
        }
        
        stage("Deploy") {
            steps {
                echo "Deploying the Container"
                sh "docker run -d -p 8000:8000 rajesh93/django-app:latest"
                sh "docker run -d -p 3000:3000 rajesh93/react-app:latest"
            }
        }
    }
}
