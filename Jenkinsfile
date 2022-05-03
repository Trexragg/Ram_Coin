ipeline {
    agent none 
    environment {
        docker_user = "linhbngo"
    }
    stages {
        stage('Publish') {
            agent {
                kubernetes {
                    inheritFrom 'agent-template'
                }
            }
            steps{
                container('docker') {
                    sh 'echo $DOCKER_TOKEN | docker login --username $DOCKER_USER --password-stdin'
                    sh 'cd rng; docker build -t $DOCKER_USER/rng:$BUILD_NUMBER .'
                    sh 'docker push $DOCKER_USER/rng:$BUILD_NUMBER'
                }
            }
        }
        stage ('Deploy') {
            agent {
                node {
                    label 'deploy'
                }
            }
            steps {
                sshagent(credentials: ['cloudlab']) {
                    sh "sed -i 's/DOCKER_REGISTRY/${docker_user}/g' rng.yaml"
                    sh "sed -i 's/BUILD_NUMBER/${BUILD_NUMBER}/g' rng.yaml"
                    sh 'scp -r -v -o StrictHostKeyChecking=no *.yaml lngo@130.127.132.246:~/'
                    sh 'ssh -o StrictHostKeyChecking=no lngo@130.127.132.246 kubectl apply -f /users/lngo/rng.yaml -n jenkins'
                    sh 'ssh -o StrictHostKeyChecking=no lngo@130.127.132.246 kubectl apply -f /users/lngo/rng-service.yaml -n jenkins'                                        
                }
            }
        }
    }
}
