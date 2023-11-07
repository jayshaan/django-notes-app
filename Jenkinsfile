pipeline {
    agent any
    
    stages {
        stage("Clone Code") {
            steps {
                echo "Cloning the code"
                git url: "https://github.com/jayshaan/django-notes-app.git", branch: "main"
            }
        }

        stage("Build") {
            steps {
                echo "Building the image"
                script {
                    // Use Dockerfile for building the image
                     def dockerTool = tool name: 'Docker', type: 'Tooltype'
                    def dockerImage = docker.build('my-note-app', '-f Dockerfile .')

                    // Tag the image
                    dockerImage.inside {
                        sh "echo 'Running some commands inside the container...'"
                    }
                }
            }
        }

        stage("Push to Docker Hub") {
            steps {
                echo "Pushing the image to Docker Hub"
                withCredentials([usernamePassword(credentialsId: "github", passwordVariable: "gitHubPass", usernameVariable: "gitHubUser")]) {
                    sh "docker login -u ${env.gitHubUser} -p ${env.gitHubPass}"

                    // Tag and push the image
                    sh "docker tag my-note-app ${env.gitHubUser}/my-note-app:latest"
                    sh "docker push ${env.gitHubUser}/my-note-app:latest"
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
