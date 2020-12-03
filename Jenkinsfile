def gv

pipeline {
    agent any
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
                    echo "building jar"
                    //gv.buildJar()
                    sh 'helm repo add brigade https://brigadecore.github.io/charts'
                    sh 'helm search repo brigade'
                    
                }
            }
        }
        stage("build image") {
            steps {
                script {
                    echo "building image"
                    //gv.buildImage()
                    withKubeConfig([credentialsId: 'k8s-credentials', serverUrl: 'https://64e7e2ab-5838-45f5-bba7-dbb7c20f0e14.eu-central-1.linodelke.net']) {
                        sh 'kubectl get nodes'
                    }

                }
            }
        }
        stage("deploy") {
            steps {
                script {
                    echo "deploying"
                    //gv.deployApp()
                }
            }
        }
    }   
}