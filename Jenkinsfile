pipeline{
    agent {label "dev"};
    stages{
        stage("Code"){
            steps{
                git url: "https://github.com/rupeshsonawane25/two-tier-flask-app", branch: "master"
            }
        }
        stage("Build"){
            steps{
                sh "docker build -t two-tier-flask-app ."
            }
        }
        stage("Test"){
            steps{
                echo "Tester test cases likh ke dega.."
            }
        }
        stage("Push to Docker Hub"){
            steps{
                withCredentials([usernamepassword(credentialsId:"dockerHubCreds",
                usernameVariable:"dockerHubUser"
                passwordVariable:"dockerHubPass"
                )]){
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker image tag two-tier-flask-app ${env.dockerHubUser}/two-tier-flask-app"
                sh "docker push ${env.dockerHubUser}/two-tier-flask-app:latest"
                }
            }
        }
        stage("Deploy"){
            steps{
                echo "docker compose up -d --build flask-app"
            }
        }
    }
}
