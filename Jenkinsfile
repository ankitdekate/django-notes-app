pipeline{
    
    agent {
        node{
            label "dev"
            
        }
    }    
    
    stages{
        stage("Clone Code"){
            steps{
                git url : "https://github.com/ankitdekate/django-notes-app.git", branch : "main"
                echo "Code Cloned"
            }
        }
        stage("Build & Test"){
            steps{
                sh "docker build . -t notes-app-jenkins:latest"
            }
        }
        stage("Push to Docker Hub"){
            steps{
                withCredentials(
                    [usernamePassword(
                        credentialsId:"dockerHubCreds",
                        passwordVariable:"dockerHubPass",
                        usernameVariable:"dockerHubUser")
                    ]
                ){
                    sh "docker image tag notes-app-jenkins:latest ${env.dockerHubUser}/notes-app-jenkins:latest"
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                    sh "docker push ${env.dockerHubUser}/notes-app-jenkins:latest"
                }
            }
        }
        stage("Deploy"){
            steps{
                // sh "docker run -d -p 8000:8000 --name notes-app notes-app:latest"
                sh "docker compose up -d"
            }
        }
    }
}
