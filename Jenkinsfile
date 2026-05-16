pipeline{
    agent { label 'dev' }
    stages{
        stage("Code Clone"){
            steps{
                git url: "https://github.com/rupeshsonawane25/two-tier-flask-app.git", branch: "master"
            }
        }
        stage("Trivy File System Scan"){
            steps{
                sh "trivy fs . -o results.json"
            }
        }
        stage("Build"){
            steps{
                sh "docker build -t two-tier-flask-app ."
            }
        }
        stage("Test"){
            steps{
                echo "Devloper/Tester test case likh ke dega.."
            }
        }
        stage("Push to Docker Hub"){
            steps{
                withCredentials([usernamePassword(
                    credentialsId:"dockerHubCreds",
                    usernameVariable: "dockerHubUser",
                    passwordVariable: "dockerHubPass"
                )]){
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker image tag two-tier-flask-app ${env.dockerHubUser}/two-tier-flask-app"
                sh "docker push ${env.dockerHubUser}/two-tier-flask-app"                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         
                }
            }
        }
        stage("Deploy"){
            steps{
                sh "docker compose up -d --build flask-app"
            }
        }
    }
    post{
        success{
            script{
                emailext 
                from: 'rrsonawane15@gmail.com',
                to: 'rrsonawane15@gmail.com',
                body: 'Build Success for demo-cicd app',
                subject: 'Build Success for demo-cicd app'
            }
        }
        failure{
            script{
                emailext attachLog: true,
                from: 'rrsonawane15@gmail.com',
                to: 'rrsonawane15@gmail.com',
                body: 'Build failed for demo-cicd app',
                subject: 'Build failed for demo-cicd app'
            }
        }
    }
}
