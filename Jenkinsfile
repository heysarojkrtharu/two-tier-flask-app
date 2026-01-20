pipeline {
    agent {label: "dev"};

    stages {
        stage('code') {
            steps {
                echo 'Cloning Code'
                git url: 'https://github.com/heysarojkrtharu/two-tier-flask-app.git', branch: 'master'
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
}
