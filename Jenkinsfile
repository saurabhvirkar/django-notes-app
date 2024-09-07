pipeline {
    agent {
        node{
            label "dev"
        }
    }
    stages{
        stage("Clone Code"){
            steps{
                echo "Run from Github jenkinfile"
                git url: "https://github.com/saurabhvirkar/django-notes-app.git", branch: "main"
            }
        }
        stage("Build & Test"){
            steps{
                sh "docker build . -t notes-app-jenkins:latest"
            }
        }
        
        stage ("Push to DockerHub")  {
            steps{
                withCredentials(
                    [usernamePassword(
                        credentialsId:"dockerHubCreds", 
                        passwordVariable:"dockerHubPass",
                        usernameVariable:"dockerHubUser"
                        )
                    ]
                ){
                sh "docker image tag notes-app-jenkins:latest ${env.dockerHubUser}/notes-app-jenkins:latest"
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker push ${env.dockerHubUser}/notes-app-jenkins:latest"
                }
            }
        }
        stage("Deploy"){
            steps {
                sh "docker compose down && docker compose up -d"
            }
        }
    }
}
