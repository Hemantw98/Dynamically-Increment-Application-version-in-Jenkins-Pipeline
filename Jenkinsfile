#!/usr/bin/env groovy

library identifier: 'jenkins-shared-library@master', retriever: modernSCM(
        [$class: 'GitSCMSource',
         remote: 'https://gitlab.com/nanuchi/jenkins-shared-library.git',
         credentialsId: 'gitlab-credentials'
        ]
)


def gv

pipeline {
    agent any
    tools {
        maven 'Maven'
    }
    stages {
        stage("increment version") {
            steps {
                script {
                    echo 'incrementing app version...'
                    sh "mvn build-helper:parse-version versions:set " +
                       "-DnewVersion=\\${parsedVersion.majorVersion}.\\${parsedVersion.minorVersion}.\\${parsedVersion.nextIncrementalVersion} " +
                       "versions:commit"
                    def matcher = readFile('pom.xml') =~ '<version>(.+)</version'
                    def version = matcher[0][1]
                    env.IMAGE_NAME = "$version-${BUILD_NUMBER}"
                }
            }
        }
        stage("init") {
            steps {
                script {
                    def gv = load "script.groovy"
                    gv()
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
                    buildImage "hemantw98/my-repo:${env.IMAGE_NAME}"
                    dockerLogin()
                    dockerPush "hemantw98/my-repo:${env.IMAGE_NAME}"
                }
            }
        }
        stage("deploy") {
            steps {
                script {
                    deployApp()
                }
            }
        }
    }
}
