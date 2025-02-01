pipeline{
    agent any

    stages{
        stage('docker install'){
            steps{ 
                script {
                    def dockerInstalled = sh(script: 'which docker', returnStatus: true) == 0
                    if (!dockerInstalled) {
                        sh '''
                        sudo yum update -y
                        sudo yum install -y docker
                        sudo systemctl start docker
                        sudo systemctl enable docker
                        sudo usermod -aG docker jenkins
                        '''
                    }
                }
            }
        }
        stage('pull'){
            steps{ 
                sh 'docker pull nginx'
            }
        }
        stage('run'){
            steps{
                sh 'docker run -it -d --name zak -p 8000:80 nginx'
            }
        }
        stage('backup'){
            steps{
                sh 'docker commit zak backup'
            }
        }
        stage('tag'){
            steps{
                sh 'docker tag backup zakeer910/jenkmock'
            }
        }
        stage('push'){
            steps{
                 sh 'echo "Zakeer@7786" | docker login -u "zakeer910" --password-stdin'
                sh 'docker push zakeer910/jenkmock'
            }
        }
    }
    post{
        always{
            sh 'docker rmi -f nginx'
        }
        success{
            sh 'docker rm -f zak'
        }
    }
    
}