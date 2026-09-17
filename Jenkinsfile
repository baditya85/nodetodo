pipeline {
    agent {label 'dev-server'}

    stages {
        stage("code") {
            steps {
                echo "code clone from git "
                git url: "https://github.com/baditya85/nodetodo.git", branch:"main"
            }
        }
        stage("build") {
            steps {
                echo " build the image"
                sh "docker build -t nodeapptest ."
            }
        }
        stage("push to docker hub") {
            steps {
                echo "push the build image"
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
                echo " deploy the build image which push to docker and now this will pull from the same and make the container"
                sh "docker compose down && docker compose up -d --build"
            }
        }
    }
}
