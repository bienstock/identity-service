node ('nimble-jenkins-slave') {
    def app
    /*stage('Clone & Build') {
        // slackSend 'Started build no. ${env.BUILD_ID} of ${env.JOB_NAME}'
        git(url: 'https://github.com/nimble-platform/identity-service.git', branch: 'master')
        sh 'git submodule init'
        sh 'git submodule update'
        withMaven(maven: 'M339') {
          sh 'mvn clean install -DskipTests'
        }
    }
    // bla
    stage ('Docker Build') {
        withMaven(maven: 'M339') {
            sh 'mvn -f identity-service/pom.xml docker:build'
        }
    }
    stage ('Docker Push')  {
        docker.withRegistry('https://registry.hub.docker.com', 'NimbelPlatformDocker') {
            withMaven(maven: 'M339') {
                sh 'mvn -f identity-service/pom.xml docker:push'
            }
        }
    }*/
    stage ('Clean WS & Clone') {
        deleteDir()
        sh 'git clone --recursive https://github.com/nimble-platform/identity-service'
    }
    stage ('Build docker image') {
        sh '''cd identity-service
              /bin/bash -xe deploy.sh docker-build'''
    }
    stage ('Push docker image') {
        withDockerRegistry([credentialsId: 'NimbleDocker']) {
            sh' docker tag nimbleplatform/identity-service:latest nimbleplatform/identity-service:bienstock'
            sh 'docker push nimbleplatform/identity-service:bienstock'
        }
    }
}

