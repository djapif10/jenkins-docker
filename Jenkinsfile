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
                withDockerRegistry([credentialsId: registryCredential]) {
                    sh "docker login -u ${DOCKER_REGISTRY_USERNAME} -p ${DOCKER_REGISTRY_PASSWORD}"
                    sh "docker push ${imageName}"
                }
            }
        }
    }
}
