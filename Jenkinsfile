pipeline {
    agent any
    
    environment {
        registryCredential = 'dockerhub-creds'
        imageName = 'fdjapi10/myubuntu'
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
                    sh "docker login -u ${username} -p ${password}"
                    sh "docker push ${imageName}"
                }
            }
        }
    }
}
