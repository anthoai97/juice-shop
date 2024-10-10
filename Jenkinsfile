pipeline {
    agent any
    
    tools {
        nodejs '18.20.4'
    }

    stages {
        stage('Example') {
            steps {
                sh 'npm version'
            }
        }
    }

}