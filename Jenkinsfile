pipeline {
    agent none
    stages {
        stage('build') {
            steps {
                script {   
                    echo "Building the application..."   
                    echo "Branch $GIT_BRANCH" 
                }
            }
        }
        stage('test') {
            when {
                expression {
                    GIT_BRANCH == 'master'
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
