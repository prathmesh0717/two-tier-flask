
pipeline {
    agent {
        label "dev"
    }

    stages {

        stage("Code Clone") {
            steps {
                git url: "https://github.com/prathmesh0717/two-tier-flask.git",
                    branch: "main"
            }
        }

        stage("Build Code") {
            steps {
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
        stage("Test Code") {
            steps {
                echo "This code is tested using SonarQube"
            }
        }

        stage("Deploy Code") {
            steps {
                sh "docker compose up -d --build flask-app"
            }
        }
    }  // stages block closed

    post {
        success{
            script{
                emailext from: 'prathmeshkarale03gmail.com',
                 to: 'prathmeshkarale03@gmail.com',
                 body: 'Build Success for demo-cicd App',
                 subject: 'Build Success for demo-cicd App'
            }
        }
        failure{
            script{
                emailext from: 'prathmeshkarale03gmail.com',
                 to: 'prathmeshkarale03@gmail.com',
                 body: 'Build Failed for demo-cicd App',
                 subject: 'Build Failed for demo-cicd App'
            }
        }
    }
}
