pipeline {
    agent any
    
    environment {
        SERVER_IP = credentials('server-ip')
    }
    
    options {
        timeout(time: 1, unit: 'HOURS')
        buildDiscarder(logRotator(numToKeepStr: '5'))
        timestamps()
        ansiColor('xterm')
        retry(3)
    }
    
    stages {   //Nested stages for learning not recommended in Production
        stage('Build and Test') {
            stages {
                stage('Install Dependencies') {
                    steps {
                        sh "pip install -r requirements.txt"
                    }
                }
                
                stage('Test Application') {
                    steps {
                        sh "pytest"
                    }
                }
                
                stage('Package Code') {
                    steps {
                        sh "zip -r myapp.zip ./* -x '*.git*'"
                        sh "ls -lart"
                    }
                }
            }
        }
        
        stage('Deploy to Ubuntu-Server') {
            steps {
                withCredentials([sshUserPrivateKey(credentialsId: 'ssh-key', keyFileVariable: 'SSH_KEY', usernameVariable: 'USERNAME')]) {
                    sh '''
                        scp -i $SSH_KEY -o StrictHostKeyChecking=no myapp.zip ${USERNAME}@${SERVER_IP}:/home/ubuntu/
                        ssh -i $SSH_KEY -o StrictHostKeyChecking=no ${USERNAME}@${SERVER_IP} << EOF
                            unzip -o /home/ubuntu/myapp.zip -d /home/ubuntu/myapp
                            source app/venv/bin/activate
                            pip install -r requirements.txt
                            sudo systemctl restart myapp.service
                        EOF
                    '''
                }
            }
        }
    }
    
    post {
        failure {
            emailext (
                subject: "Pipeline Failed: ${currentBuild.fullDisplayName}",
                body: "Pipeline failed at stage: ${currentBuild.description}",
                to: 'team@example.com'
            )
        }
        success {
            echo 'Pipeline completed successfully!'
        }
        always {
            echo 'Pipeline execution completed'
        }
    }
}
