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
                    sh 'scp -r -v -o StrictHostKeyChecking=no *.yaml tr950723@155.98.37.80:~/'
                    sh 'ssh -o StrictHostKeyChecking=no tr950723@155.98.37.80 kubectl apply -f /users/tr950723/redis.yaml -n jenkins'
                    sh 'ssh -o StrictHostKeyChecking=no tr950723@155.98.37.80 kubectl apply -f /users/tr950723/redis-service.yaml -n jenkins'                                        
                }
            }
        }
    }
}
