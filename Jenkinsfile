pipeline {
    
    agent any
    
    stages{
        stage("Clone Code"){
            steps{
                git url: "https://github.com/RithendSushanth/todo-cicd-jenkins.git", branch: "master"
            }
        }
        stage("Build & Test"){
            steps{
                sh "docker build -t notes-app:latest ."
            }
        }
        stage("Push to DockerHub"){
            steps{
                withCredentials(
                    [usernamePassword(
                        credentialsId:"dockerHub",
                        passwordVariable:"dockerHubPass", 
                        usernameVariable:"dockerHubUser"
                        )
                    ]
                ){
                sh "docker image tag notes-app:latest ${env.dockerHubUser}/notes-app:latest"
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker push ${env.dockerHubUser}/notes-app:latest"
                }
            }
        }
        
        stage("Deploy"){
            steps{
                sh "docker-compose down && docker-compose up -d "
            }
        }
    }
}