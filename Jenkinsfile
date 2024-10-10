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

        // stage('trivy fs scan') {
        //     steps {
        //         sh "trivy fs --format table -o fs.html ."
        //     }
        // }

        stage('git leak fs scan') {
            steps {
                sh "gitleaks dir"
            }
        }
    }

}