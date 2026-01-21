
@Library('Shared') _

pipeline {
    agent {
        label "dev"
        }

    stages {
        stage('code') {
            steps {
                echo 'Cloning Code'

                clone('https://github.com/heysarojkrtharu/two-tier-flask-app.git',  'master')
            }
        }

        stage("Trivy File System Scanned"){
            steps {
                echo ' Scanning The File System By Trivy '
                trivy_scan()
            }
        }

        stage('build') {
            steps {
                echo 'Building Projects using Docker'
                docker_build("two-tier-flask-app")
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

                docker_push("two-tier-flask-app", "dockerHubCreds")
                
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
