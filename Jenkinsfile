pipeline{
    agent { label "dev"};
    stages{
        stage("code clone"){
           steps{
               git url: "https://github.com/flexis007/two-tier-flask-app.git", branch:"master"
           } 
        }
        stage("code build"){
           steps{
               sh "docker build -t two-tier-flaskapp:latest ."
           } 
        }
        stage("Push to docker hub"){
            steps{
                withCredentials([usernamePassword(
                    credentialsId: "dockerhub",
                    usernameVariable: "dockeruser",
                    passwordVariable: "dockerpass"
                    )])
                    {
                        sh "docker login -u ${env.dockeruser} -p ${env.dockerpass}"
                        sh "docker image tag two-tier-flaskapp ${env.dockeruser}/two-tier-flaskapp:$v{BUILD_NUMBER} "
                        sh "docker push ${env.dockeruser}/two-tier-flaskapp:$v{BUILD_NUMBER} "
                    }
            }
        }
        stage("code deploy"){
            steps{
                 
                sh "docker compose up -d --build flask-app"
            }
        }
    }
}
