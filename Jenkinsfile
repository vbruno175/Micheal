pipeline {
    agent any
    environment{
        DOCKER_TAG = getDockerTag()
    }
    stages{
        stage('Build Docker Image'){
            steps{
                sh "podman build . -t vbruno175/vbruno175:${DOCKER_TAG} --tls-verify=false"
            }
        }
        stage('DockerHub Push'){
            steps{
                sh "podman push vbruno175/vbruno175:${DOCKER_TAG} --tls-verify=false"
                }
        }
        stage('Deploy to DevServer'){
            steps{
		sh "podman run -d -p 8089:8080 --name=vbruno175 vbruno175/vbruno175:${DOCKER_TAG} --tls-verify=false"
            }
        }
    }
}

def getDockerTag(){
    def tag  = sh script: 'git rev-parse HEAD', returnStdout: true
    return tag
}
