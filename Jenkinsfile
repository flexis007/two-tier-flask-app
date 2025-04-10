pipeline{
    agent { label "dev"};
    stages{
        stage("code clone"){
           steps{
               git url: "https://github.com/flexis007/two-tier-flask-app.git", branch:"master"
           } 
        }
        stage("trivy scan"){
            steps{
                sh " trivy fs . -o results.json"
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
                        sh "docker image tag two-tier-flaskapp ${env.dockeruser}/two-tier-flaskapp:v${BUILD_NUMBER} "
                        sh "docker push ${env.dockeruser}/two-tier-flaskapp:v${BUILD_NUMBER} "
                    }
            }
        }
        stage("code deploy"){
            steps{
                 
                sh "docker compose up -d --build flask-app"
            }
        }
    }
    post{
        success{
            script{
                emailext from: 'imti.ansari007@gmail.com',
                to: 'imti.ansari007@gmail.com',
                body: 'Build success for Demo CICD App',
                subject: 'Build success for Demo CICD App'
            }
        }
        failure{
            script{
                emailext from: 'imti.ansari007@gmail.com',
                to: 'imti.ansari007@gmail.com',
                body: 'Build Failed for Demo CICD App',
                subject: 'Build Failed for Demo CICD App'
            }
        }
    }
}
