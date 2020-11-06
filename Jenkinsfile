pipeline {
    agent none
    stages {
        stage('build') {
            steps {
                script {   
                    echo "Building the application..."   
                    echo "Branch $env.BRANCH_NAME" 
                }
            }
        }
        stage('test') {
            when {
                expression {
                    env.BRANCH_NAME == 'master'
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
