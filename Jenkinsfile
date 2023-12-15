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
                    def dockerCmd = 'docker run -p 3080:3080 -d chadunelib/my-repository:1.0'
                    sshagent(['ec2-server-key']) {
                        sh "ssh -o StictHostKeyChecking=no ec2-user@35.180.97.166 ${dockerCmd}"
                    }
                }
            }
        }               
    }
} 
