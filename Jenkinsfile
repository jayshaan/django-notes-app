pipeline {
    agent any 
    
    stages{
        stage("Clone Code"){
            steps {
                echo "Cloning the code"
                git url: "https://github.com/jayshaan/django-notes-app.git", branch: "main"
            }
        }
        stage("Build"){
            steps {
                script { 
                    docker.build('my-note-app')
                }
                echo "Building the image"
                sh "docker build -t my-note-app ."
            }
        }
        stage("Push to Docker Hub"){
            steps {
                echo "Pushing the image to docker hub"
                withCredentials([usernamePassword(credentialsId:"github",passwordVariable:"gitHubPass",usernameVariable:"gitHubUser")]){
                sh "docker tag my-note-app ${env.gitHubUser}/my-note-app:latest"
                sh "docker login -u ${env.gitHubUser} -p ${env.gitHubPass}"
                sh "docker push ${env.gitHubUser}/my-note-app:latest"
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
