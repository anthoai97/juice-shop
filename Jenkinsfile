pipeline {
    agent any
    
    tools {
        nodejs '18.20.4'
    }

    environment {
        PASS_SECURITY = "true"
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

        stage('Git leak fs scan') {
            steps {
                script {
                    def exitcode = sh(script: '''
                        alias gitleaks="gitleaks dir --verbose --exit-code 183 --no-banner --no-color --report-format json --report-path secrets.json"
                        gitleaks 2> ./gitleaks-summary.txt
                        echo $?
                    ''', returnStdout: true).trim()

                    echo "Gitleaks exit code: ${exitcode}"
                    if (exitcode == "1") {
                        security_check = "false"  // Security check fails
                        echo "Security check failed due to Gitleaks!"
                    } else {
                        security_check = "true"  // Security check passes
                        echo "Security check passed!"
                    }
                }
                
            }
        }

        stage('Notification') {
            steps {
                script {
                    // Use the updated security_check variable for notification
                    if (security_check == "true") {
                        echo "Security check passed! Sending success notification..."
                        // Simulate success notification
                    } else {
                        echo "Security check failed! Sending failure notification..."
                        // Simulate failure notification
                    }
                }
            }
        }
    }

}