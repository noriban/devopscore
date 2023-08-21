def frontendImage="noriban/frontend"
def backendImage="noriban/backend"
def backendDockerTag=""
def frontendDockerTag=""
def dockerRegistry=""
def registryCredentials="Dockerhub"

pipeline {
    agent {
        label 'agent'
    }
    tools {
        terraform 'terraform'
    }
}

// parameters {
//   string(description: 'backend docker image tag', name: 'backendDockerTag' defaultValue: '')
//   string(description: 'frontend docker image tag', name: 'frontendDockerTag' defaultValue: '')
// }

stages {
    stage('Get Code') {
        steps {
            checkout scm
        }
    }
    stage('Clean running containers') {
        steps {
            sh "docker rm -f frontend backend"
        }
    }
    stage('Adjust version') {
        steps {
            script{
                backendDockerTag = params.backendDockerTag.isEmpty() ? "latest" : params.backendDockerTag
                frontendDockerTag = params.frontendDockerTag.isEmpty() ? "latest" : params.frontendDockerTag
                
                currentBuild.description = "Backend: ${backendDockerTag}, Frontend: ${frontendDockerTag}"
            }
        }
    }
    stage('Deploy application') {
        steps {
            script {
                withEnv(["FRONTEND_IMAGE=$frontendImage:$frontendDockerTag", 
                            "BACKEND_IMAGE=$backendImage:$backendDockerTag"]) {
                    docker.withRegistry("$dockerRegistry", "$registryCredentials") {
                        sh "docker-compose up -d"
                    }
                }
            }
        }
    }
}