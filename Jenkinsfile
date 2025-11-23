pipeline{
    agent any
    stages{
        stage("Code"){
            steps{
                git url : "https://github.com/paragtelang86/two-tier-flask-app.git"
            }
        }
        stage("Build"){
            steps{
                sh "docker build -t two-tier-flask-app:latest ."
            }
        }
        stage("push"){
            steps{
                withCredentials([usernamePassword(
                    credentialsId:"DockerCred",
                    passwordVariable:"DockerhubPass",
                    usernameVariable:"DockerhubUser")]){
                     
                sh "docker login -u ${env.DockerhubUser} -p ${env.DockerhubPass}"
                sh "docker image tag two-tier-flask-app  ${env.DockerhubUser}/two-tier-flask-app"
                sh "docker push ${env.DockerhubUser}/two-tier-flask-app:latest"   
                    }
            }
        }
        stage("Deploy"){
            steps{
                sh "docker compose up -d --build flask-app"
            }
        }
    }
}
