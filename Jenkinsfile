// pipeline {
//     agent any
//     tools {nodejs "node"}
//     environment{
//         imageName = "react-test"
//         registryCredintials = "dockerhub"
//         dockerImage = ''
//     }
//     stages {
//         stage ('Install Dependencies') {
//             steps{
//                 sh 'npm install'
//             }
//         }
//         stage ('Test') {
//             steps{
//                 sh 'npm test'
//             }
//         }
//     }
//     stage("bulding Image"){
//         steps{
//             script{
//                 dockerImage = docker.build imageName
//             }
//         }
//     }
//     stage("Deploy Image"){
//         steps{
//             script{
//                 docker.withRegistry( 'https://registry.hub.docker.com', "dockerhub-creds" ){
//                     dockerImage.push("${env.BUILD_NUMBER}")
//                 }
//             }
//         }
//     }
// }

pipeline {
    agent any
    tools {nodejs "node"} // Assuming you've configured NodeJS tool in Jenkins

    environment {
        imageName = "react-test"
        localRegistry = "localhost:5000" // Assuming your local Docker registry runs on port 5000
        dockerImage = ''
    }

    stages {
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Test') {
            steps {
                sh 'npm test'
            }
        }

        stage("Building Image") {
            steps {
                script {
                    // Building Docker image
                    dockerImage = docker.build(imageName)
                }
            }
        }

        stage("Deploy Image to Local Registry") {
            steps {
                script {
                    // Tag the built image for the local registry
                    dockerImage.tag("$localRegistry/$imageName:${env.BUILD_NUMBER}")
                    
                    // Push the tagged image to the local registry
                    dockerImage.push("${env.BUILD_NUMBER}")
                }
            }
        }

        stage("Deploy Image to Local Machine") {
            steps {
                script {
                    // Pull the image from the local registry to your local Docker daemon
                    sh "docker pull $localRegistry/$imageName:${env.BUILD_NUMBER}"
                }
            }
        }
    }
}
