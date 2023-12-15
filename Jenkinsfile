#!/usr/bin/env groovy

@Library('jenkins-shared-library')
def gv

pipeline {
    agent any
    tools {
        maven "maven-3.6"
    }
    stages {
        stage("init") {
            steps {
                script {
                    gv = load "script.groovy"
                }
            }
        }
        stage("build jar") {
            steps {
                script {
                    buildJar()
                }
            }
        }
        stage("build and push image") {
            steps {
                script {
                    buildImage 'chadunelib/my-repository:jma-4.0'
                    dockerLogin()
                    dockerPush 'chadunelib/my-repository:jma-4.0'
                }
            }
        }
        stage("deploy") {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'docker-hub-repository', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                        sh "echo $PASSWORD | docker login -u $USERNAME --password-stdin"
                    }
                    def dockerCmd = 'docker run -p 3080:3080 -d chadunelib/my-repository:1.0'
                    sshagent(['ec2-server-key']) {
                        sh "ssh -o StrictHostKeyChecking=no ec2-user@35.180.97.166 ${dockerCmd}"
                    }
                }

            }
        }
    }

}
