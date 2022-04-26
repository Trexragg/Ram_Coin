pipeline {
    agent none 
    stages {
        stage ('Deploy') {
            agent {
                node {
                    label 'deploy'
                }
            }
            steps {
                sshagent(credentials: ['cloudlab']) {
                    sh 'scp -r -v -o StrictHostKeyChecking=no *.yaml lngo@130.127.132.246:~/'
                    sh 'ssh -o StrictHostKeyChecking=no lngo@130.127.132.246 kubectl apply -f /users/lngo/redis.yaml -n jenkins'
                    sh 'ssh -o StrictHostKeyChecking=no lngo@130.127.132.246 kubectl apply -f /users/lngo/redis-service.yaml -n jenkins'                                        
                }
            }
        }
    }
}
