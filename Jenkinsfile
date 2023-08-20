pipeline {
    agent any
    
    stages{
        stage("Clone code"){
            steps {
                echo "cloning the code"
                git url:"https://github.com/roshanupare/django-app-cicd.git", branch: "main"
            }
        }
        
        stage("Build"){
            steps {
                echo "Building the image"
                sh "docker build -t note-app ."
            }
        }
        stage("Push to the Dockerhub"){
            steps {
                echo "Pushing the image"
                withCredentials([usernamePassword(credentialsId:"dockerhub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                sh "docker tag note-app ${env.dockerHubUser}/note-app:latest"
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker push ${env.dockerHubUser}/note-app:latest"
                }
            }
        }
        stage("Deploy"){
            steps {
                echo "Deploy the container"
                sh "docker-compose down && docker-compose up -d"
            }
        }
        
    }
}
