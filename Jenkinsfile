pipeline {
    agent none 
    environment {
        docker_user = "tr950723"
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
                    sh 'cd worker; docker build -t $DOCKER_USER/worker:$BUILD_NUMBER .'
                    sh 'docker push $DOCKER_USER/worker:$BUILD_NUMBER'
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
                    sh "sed -i 's/DOCKER_REGISTRY/${docker_user}/g' worker.yaml"
                    sh "sed -i 's/BUILD_NUMBER/${BUILD_NUMBER}/g' worker.yaml"
                    sh 'scp -r -v -o StrictHostKeyChecking=no *.yaml tr950723@155.98.37.78:~/'
                    sh 'ssh -o StrictHostKeyChecking=no tr950723@155.98.37.78 kubectl apply -f /users/tr950723/worker.yaml -n jenkins'
                }
            }
        }
    }
}
