pipeline {
    agent any
    environment{
        DOCKER_TAG = getDockerTag()
    }
    stages{
        stage('Build Docker Image'){
            steps{
                sh "podman build . -t kammana/node-app:${DOCKER_TAG} "
            }
        }
        stage('DockerHub Push'){
            steps{
                withCredentials([string(credentialsId: 'docker-hub', variable: 'dockerHubPwd')]) {
                    sh "podman login -u kammana -p ${dockerHubPwd}"
                    sh "podman push kammana/node-app:${DOCKER_TAG}"
                }
            }
        }
        stage('Deploy to kubernetes'){
            steps{
		sh "chmod +x changeTag.sh"
		sh "./changeTag.sh ${DOCKER_TAG}"   
		sh "kubectl apply -f node-app-pod.yml"
		sh "kubectl apply -f services.yml"    
            }
        }
    }
}

def getDockerTag(){
    def tag  = sh script: 'git rev-parse HEAD', returnStdout: true
    return tag
}
