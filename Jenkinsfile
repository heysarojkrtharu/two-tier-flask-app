pipeline {
    agent {
        label "dev"
        }

    stages {
        stage('code') {
            steps {
                echo 'Cloning Code'
                git url: 'https://github.com/heysarojkrtharu/two-tier-flask-app.git', branch: 'master'
            }
        }

        stage("Trivy File System Scanned"){
            steps {
                echo ' Scanning The File System By Trivy '
                sh 'trivy fs . -o results.json'
            }
        }

        stage('build') {
            steps {
                echo 'Building Projects using Docker'
                sh 'docker build -t two-tier-flask-app .'
            }
        }

        stage('test') {
            steps {
                echo 'testing / Developer Tester'
            }
        }
        stage("push to dockerhub"){
            steps {
                echo " Pushing to docker hub "
                withCredentials(
                    [usernamePassword(
                        credentialsId: "dockerHubCreds",
                        passwordVariable:"dockerHubPass" ,
                        usernameVariable:"dockerHubUser" )]
                    ){
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                    sh "docker image tag two-tier-flask-app ${env.dockerHubUser}/two-tier-flask-app"
                    sh " docker push ${env.dockerHubUser}/two-tier-flask-app:latest "
                }
                
            }
        }

        stage('deploy') {
            steps {
                echo 'Deploying Docker compose up'
                sh 'docker compose up -d --build flask-app'
            }
        }
    }
    
    post{
        success{
            script{
                emailext from: 'sarojc11345@gmail.com',
                to: 'sarojc11345@gmail.com',
                body: 'Build success for Demo CICD App',
                subject: 'Build success for Demo CICD App'
            }
        }
        failure{
            script{
                emailext from: 'sarojc11345@gmail.com',
                to: 'sarojc11345@gmail.com',
                body: 'Build Failed for Demo CICD App',
                subject: 'Build Failed for Demo CICD App'
            }
        }
    }
}
