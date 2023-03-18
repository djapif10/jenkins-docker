pipeline {
    agent any
    
    environment {
        registry = 'docker.io'
        registryCredential = 'dockerhub-creds'
        imageName = 'fdjapi10/myubuntu'
        containerName = 'my-container'
    }
    
    stages {
        stage('Build') {
            steps {
                checkout scm
                sh "docker build -t ${imageName} ."
            }
        }
        
        stage('Push to DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: registryCredential, passwordVariable: 'password', usernameVariable: 'username')]) {
                    withDockerRegistry([credentialsId: registryCredential, url: "https://${registry}"]) {
                        sh "echo ${password} | docker login -u ${username} --password-stdin ${registry}"
                        sh "docker push ${imageName}"
                    }
                }
            }
        }
        
        stage('Deploy') {
            steps {
                withDockerRegistry([credentialsId: registryCredential, url: "https://${registry}"]) {
                    sh "docker pull ${imageName}"
                    sh "docker stop ${containerName} || true"
                    sh "docker rm ${containerName} || true"
                    sh "docker run -d --name ${containerName} ${imageName}"
                }
            }
        }
    }
}
