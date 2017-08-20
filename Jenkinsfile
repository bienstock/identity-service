node ('nimble-jenkins-slave') {
    def app
    stage('Clone & Build') {
        // slackSend 'Started build no. ${env.BUILD_ID} of ${env.JOB_NAME}'
        git(url: 'https://github.com/bienstock/identity-service.git', branch: 'check_postgres_fix')
        sh 'git submodule init'
        sh 'git submodule update'
    }

    /*stage ('Clean WS & Clone') {
        deleteDir()
        sh 'git clone --recursive https://github.com/nimble-platform/identity-service'
    }*/
    stage ('Build docker image') {
        sh '''/bin/bash -xe deploy.sh docker-build'''
    }
    stage ('Push docker image') {
        withDockerRegistry([credentialsId: 'NimbleDocker']) {
            sh' docker tag nimbleplatform/identity-service nimbleplatform/identity-service:bienstock'
            sh 'docker push nimbleplatform/identity-service:bienstock'
        }
    }
    stage ('Deploy') {
        sh '''kubectl apply -f kubernetes/deploy.yaml -n prod --validate=false'''
    }
}

