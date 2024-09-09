pipeline {
    agent any
    environment{
        DOCKER_TAG = getDockerTag()
    }
    stages{
        stage('Build Docker Image'){
            steps{
                sh "podman build . -t 192.168.229.128:31320/vbruno175:${DOCKER_TAG}"
            }
        }
        stage('DockerHub Push'){
            steps{
                sh "podman push 192.168.229.128:31320/vbruno175:${DOCKER_TAG}"
                }
        }
        stage('apply to kubernetes'){
            steps{
		sh "chmod +x changeTag.sh"
		sh "./changeTag.sh ${DOCKER_TAG}"
		sh "rm -rf pods.yml buildspec.yml"    
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
