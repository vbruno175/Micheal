pipeline {
     agent any
    environment{
        DOCKER_TAG = getDockerTag()
    }
    stages{
        stage('Build Docker Image'){
            steps{
                sh "podman build . -t 192.168.233.133:32118/vbruno175:${DOCKER_TAG}"
            }
        }
        stage('DockerHub Push'){
            steps{
                sh "podman push 192.168.233.133:32118/vbruno175:${DOCKER_TAG}"
                }
        }
        stage('change Tag'){
            steps{
		sh "chmod +x changeTag.sh"
		sh "./changeTag.sh ${DOCKER_TAG}"
            }
        }
    }	    
}

def getDockerTag(){
    def tag  = sh script: 'git rev-parse HEAD', returnStdout: true
    return tag
}
