pipeline {
    agent any

    stages {
        stage('Initialize') {
            steps {
                script {
                    env.DOCKER_TAG = getDockerTag()
                }
            }
        }
    }
        stage('Build Docker Image') {
            steps {
                sh "podman build . -t localhost:31320/vbruno175:${DOCKER_TAG}"
            }
        }

        stage('DockerHub Push') {
            steps {
                sh "podman push localhost:31320/vbruno175:${DOCKER_TAG}"
            }
        }

        stage('Apply to Kubernetes') {
            steps {
                sh "chmod +x changeTag.sh"
		sh "./changeTag.sh ${DOCKER_TAG}"
		sh "rm -rf pods.yml buildspec.yml"    
		sh "kubectl apply -f node-app-pod.yml"
		sh "kubectl apply -f services.yml"
        }
    }
}



def getDockerTag() {
    return sh(script: 'git rev-parse HEAD', returnStdout: true).trim()
}
