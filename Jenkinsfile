pipeline {
    agent any
    
    stages{
        stage("Code"){
            steps{
                git url: "https://github.com/jaivik001/node-todo-cicd", branch: "master"
            }
        }
        stage("Build & Test"){
            steps{
                sh "docker build . -t node-todo"
            }
        }
        stage("Push to DockerHub"){
            steps{
                withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                    sh "docker tag node-todo ${env.dockerHubUser}/node-todo:latest"
                    sh "docker push ${env.dockerHubUser}/node-todo:latest" 
                }
            }
        }
        stage("Deploy"){
            steps{
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
