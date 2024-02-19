pipeline{
    agent any
    
    stages{
        stage("Clone Code"){
            steps{
                echo "Cloning the code from GitHub"
                git url: "https://github.com/Ashutosh-Upadhyay1/django-notes-app.git", branch: "main"
            }
        }
        stage("Build Image"){
            steps{
                echo "Building the image using Docker"
                sh "docker build -t my-note-app ."
            }
        }    
        stage("Push to Dockerhub"){
            steps{
                echo "Pushing the image to Docker-Hub"
                withCredentials([usernamePassword(credentialsId:"dockerHub-creds",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                    sh "docker tag my-note-app ${env.dockerHubUser}/my-note-app:latest"
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                    sh "docker push ${env.dockerHubUser}/my-note-app:latest"
                }
            }
        }
        stage("Deploy"){
            steps{
                echo "Deploying the container on port"
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
