pipeline {
    agent {label 'dev-server'}

    stages {
        stage("code") {
            steps {
                git url: "https://github.com/baditya85/nodetodo.git", branch:"main"
            }
        }
        stage("build") {
            steps {
                sh "docker build -t nodeapptest ."
            }
        }
        stage("push to docker hub") {
            steps {
                withCredentials([usernamePassword(
                    credentialsId:"dockerhubcred", 
                    usernameVariable:"dockerHubUser", 
                    passwordVariable:"dockerHubPass")]){
                  sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                  sh "docker image tag nodeapptest:latest ${env.dockerHubUser}/nodeapptest:latest"
                  sh "docker push ${env.dockerHubUser}/nodeapptest:latest"  
                }
            }
        }
        stage("deploy") {
            steps {
                sh "docker compose down && docker compose up -d --build"
            }
        }
    }
}
