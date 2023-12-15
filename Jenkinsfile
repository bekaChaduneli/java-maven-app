pipeline {   
    agent any
    stages {
        stage("test") {
            steps {
                script {
                    echo "Testing the application...."
                }
            }
        }
        
        stage("build") {
            steps {
                script {
                    echo "Building the application...."
                }
            }
        }

        stage("deploy") {
            steps {
                script {
                    def dockerCmd = 'docker pull chadunelib/my-repository:jma-5.0 && docker run -p 3080:3080 -d chadunelib/my-repository:jma-5.0'
                    
                    withCredentials([usernamePassword(credentialsId: 'docker-hub-repository', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                        sshagent(['ec2-server-key']) {
                            sh "docker login -u $USERNAME -p $PASSWORD && ssh -o StrictHostKeyChecking=no ec2-user@35.180.97.166 ${dockerCmd}"
                        }
                    }
                }
            }
        }              
    }
} 
