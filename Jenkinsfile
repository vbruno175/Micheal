pipeline {
    agent any
    environment{
        DOCKER_TAG = getDockerTag()
    }
    stages{
        stage('Build Docker Image'){
            steps{
                sh "podman build . -t localhost:31320/vbruno175:${DOCKER_TAG}"
            }
        }
        stage('DockerHub Push'){
            steps{
                sh "podman push localhost:31320/vbruno175:${DOCKER_TAG}"
                }
        }
        stage('apply to kubernetes'){
            steps{
		sh "chmod +x changeTag.sh"   
            }
        }
    }	    
}

def getDockerTag(){
    def tag  = sh script: 'git rev-parse HEAD', returnStdout: true
    return tag
}
