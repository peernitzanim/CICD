pipeline {
    agent any
    environment {
        EC2_HOST = "3.237.186.147"
        SSH_USER = "ubuntu"
    }
    stages {
        stage('Git pull') {
            steps {
                sshagent(['peer_computer']) {
                    script {
                        sh """
                            ssh -o StrictHostKeyChecking=no ${SSH_USER}@${EC2_HOST} << 'EOF'
                            cd /home/ubuntu/test/
                            sudo git pull https://github.com/peernitzanim/CICD.git
                        """
                    }
                }
            }
        }
        stage('remove claster') {
            steps {
                sshagent(['peer_computer']) {
                    script {
                        sh """
                            ssh -o StrictHostKeyChecking=no ${SSH_USER}@${EC2_HOST} << 'EOF'
                             sudo docker stop hello-world-container-new
                             sudo docker rm hello-world-container-new
                            """
                    }
                }
            }
        }
        stage('remove image') {
            steps {
                sshagent(['peer_computer']) {
                    script {
                        sh """
                            ssh -o StrictHostKeyChecking=no ${SSH_USER}@${EC2_HOST} << 'EOF'
                            sudo docker rmi peernitzanim/hello-world:latest -f
                           """
                    }
                }
            }
        }
        stage('build image') {
            steps {
                sshagent(['peer_computer']) {
                    script {
                        sh """
                            ssh -o StrictHostKeyChecking=no ${SSH_USER}@${EC2_HOST} << 'EOF'
                            cd /home/ubuntu/test/
                            sudo docker build -t peernitzanim/hello-world:latest .
                           """
                    }
                }
            }
        }
         stage('create container') {
            steps {
                sshagent(['peer_computer']) {
                    script {
                        sh """
                            ssh -o StrictHostKeyChecking=no ${SSH_USER}@${EC2_HOST} << 'EOF'
                            sudo docker run -d --name hello-world-container-new -p 8080:8080 peernitzanim/hello-world:latest
                           """
                    }
                }
            }
        }
        
    }
}



