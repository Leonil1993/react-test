pipeline {
    agent any
    tools {nodejs "node"}
    environment{
        imageName = "react-app"
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
                    // docker.withServer('npipe://./pipe/docker_engine'){
                    // }
                        dockerImage.push("${env.BUILD_NUMBER}")
                }
            }
        }
    }
}


// pipeline {
//     agent any

//     environment {
//         DOCKER_HOST = 'tcp://docker-host:2375' // Replace 'docker-host' with the hostname or IP address of your Docker daemon
//     }

//     stages {
//         stage('Checkout') {
//             steps {
//                 checkout scm
//             }
//         }

//         stage('Build') {
//             steps {
//                 script {
//                     // Build Docker image
//                     docker.build('react-test')
//                 }
//             }
//         }

//         stage('Test') {
//             steps {
//                 // Run tests (assuming npm test)
//                 sh 'npm test'
//             }
//         }

//         stage('Deploy') {
//             steps {
//                 script {
//                     // Run Docker container from the built image
//                     docker.image('react-test').run('-p 8080:80 -d')
//                 }
//             }
//         }
//     }

//     post {
//         always {
//             // Clean up Docker resources
//             script {
//                 docker.image('react-test').stop()
//                 docker.image('react-test').remove()
//             }
//         }
//     }
// }
