#!/usr/bin/env groovy

library identifier: 'jenkins-shared-library@master', retriver: modernSCM(
        [$class: 'GitSCMSource'],
        remote: 'https://github.com/bekaChaduneli/jenkins-shared-library.git',
        credentialsId: 'bekaChaduneli'
)

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
                    buildImage 'chadunelib/my-repository:jma-3.0'
                    dockerLogin()
                    dockerPush 'chadunelib/my-repository:jma-3.0'
                }
            }
        }
        stage("deploy") {
            steps {
                script {
                    gv.deployApp()
                }
            }
        }
    }

}
