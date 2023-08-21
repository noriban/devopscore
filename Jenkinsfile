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
}