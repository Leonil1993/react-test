pipeline {
    agent any
    tools {nodejs "node"}
    environment{
        imageName = "react-test"
        registryCredential = "dockerhub"
        dockerImage = ''
    }
    stages {
        stage ('Install Dependencies') {
            steps{
                sh 'npm install'
            }
        }
        stage ('Test') {
            steps{
                sh 'npm test'
            }
        }
        stage ("bulding Image"){
            steps{
                script{
                    dockerImage = docker.build imageName
                }
            }
        }
        stage ("Deploy Image"){
            steps{
                script{
                    docker.withRegistry( 'https://registry.hub.docker.com', "dockerhub-creds" ){
                        dockerImage.push("${env.BUILD_NUMBER}")
                    }
                }
            }
        }
    }
}