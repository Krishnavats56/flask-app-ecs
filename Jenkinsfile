pipeline{
    agent any;
    
    stages{
        stage("code"){
            steps{
                git url: "https://github.com/Krishnavats56/flask-app-ecs.git", branch: "main"
            }
        }
        stage("build"){
            steps{
                sh "docker build -t flask-app ."
            }
        }
        stage("push to dockerhub"){
            steps{
                withCredentials([usernamePassword(
                    credentialsId:"dockerhubcreds",
                    passwordVariable:"dockerhubPass",
                    usernameVariable:"dockerhubUser"
                    )]){
                        sh "docker login -u ${env.dockerhubUser} -p ${env.dockerhubPass}"
                        sh "docker image tag flask-app ${env.dockerhubUser}/flask-app:latest"
                        sh "docker push ${env.dockerhubUser}/flask-app:latest"
                    
                }
            }
        }
        stage("deploy"){
            steps{
                sh "docker compose up -d"
            }
        }
    }
}
