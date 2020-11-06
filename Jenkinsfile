pipeline {
    agent none
    stages {
        stage('build') {
            steps {
                script {   
                    echo "Building the application..."   
                    echo "Branch $env.GIT_BRANCH" 
                }
            }
        }
        stage('test') {
            when {
                expression {
                    env.GIT_BRANCH == 'master'
                }
            }
            steps {
                script {
                    echo "Testing the application..."
                }
            }
        }
        stage('deploy') {
            steps {
                script {
                    echo "Deploying the application..."
                }
            }
        }
    }
}
