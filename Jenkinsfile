pipeline{
    agent any
    stages{
        stage("code clone"){
            steps{
                git url: "https://github.com/prathmesh0717/two-tier-flask.git", branch: "main"
            }
        }
        stage("Build Code"){
            steps{
                sh "docker build -t two-tier-flask-app ."
            }
        }
        stage("Push To Docker-Hub"){
            steps{
                withCredentials([usernamePassword(
                    credentialsId: "dockerHubCreds",
                    passwordVariable: "dockerHubPass",
                    usernameVariable: "dockerHubUser"
                )]){
            sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
            sh "docker image tag two-tier-flask-app ${env.dockerHubUser}/two-tier-flask-app"
            sh "docker push ${env.dockerHubUser}/two-tier-flask-app:latest"
                    
                }
            }
        }
        stage("test code"){
            steps{
                echo "this code test uing sonarqube"
            }
        }
        stage("Deploy code"){
            steps{
                echo "deploy code using K8s"
            }
        }
    }
}
