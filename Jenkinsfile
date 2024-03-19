pipeline {
    agent any
    stages {
        stage("Code") {
            steps {
                echo "Cloning the code"
                git url: "https://github.com/GauriKothekarnetsmartz/Django-cicd.git", branch: "pre-prod"
            }
        }
        stage("Build") {
            steps {
                echo "Building the image"
                sh "docker build -t my-food-app ."
            }
        }
        stage("Push to Docker Hub") {
            steps {
                echo "Pushing to Docker Hub"
                withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]) {
                    sh "echo 'Docker Hub Username: ${env.dockerHubUser}'"
                    sh "echo 'Docker Hub Password: ${env.dockerHubPass}'"
                    sh " docker tag my-food-app ${env.dockerHubUser}/my-food-app:latest"
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                    sh "docker push ${env.dockerHubUser}/my-food-app:latest"
                }
            }
        }
        stage("Deploy the container") {
            steps {
                echo "Deploying the container"
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
