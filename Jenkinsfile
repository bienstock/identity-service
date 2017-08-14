node ('nimble-jenkins-slave') {
    def app
    stage('Clone & Build') {
        // slackSend 'Started build no. ${env.BUILD_ID} of ${env.JOB_NAME}'
        git(url: 'https://github.com/bienstock/identity-service.git', branch: 'master')
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
    stage ('Test Deployment') {
        sh '''
                i=0
                function readinessTest ()
                {
                    readyReplicas=$(kubectl get deploy --namespace=prod identity-service  -o jsonpath='{.status.updatedReplicas})
                    if [[ $readyReplicas -ge 1 ]]
                    then
                      echo "Deployed Successfully"
                      exit 0
                    elif [[ $i -gt 60 ]]
                    then
                      exit 1
                    else
                      sleep 5
                      (( i++ ))
                      readinessTest
                    fi
                }

                readinessTest'''
    }
}

