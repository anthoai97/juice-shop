pipeline {
    agent any
    
    tools {
        nodejs '18.20.4'
    }

    environment {
        // Set your DefectDojo API and URL
        DEFECTDOJO_URL = 'http://nginx:8080'
        ENGAGEMENT_ID = '1'  // Set your Engagement ID in DefectDojo
    }

    stages {
        stage('Initialize') {
            steps {
                script {
                    sh('npm version')
                    def security_check = "true"
                    echo "Default security_check value: ${security_check}"
                }
            }
        }

        stage('Gitleaks fs scan') {
            steps {
                script {
                    def exitcode = sh(script: '''
                        alias gitleaks="gitleaks dir --redact=80 --no-banner --no-color --report-format json --report-path gitleaks-report.json"
                        gitleaks 2> ./gitleaks-summary.txt
                        echo $?
                    ''', returnStatus: true)

                    // echo "Gitleaks exit code: ${exitcode}"
                    if (exitcode == 1) {
                        security_check = "false"  // Security check fails
                        echo "Security check failed due to Gitleaks!"
                        // Send report to DEFECT DOJO
                    } else {
                        security_check = "true"  // Security check passes
                        echo "Security check passed!"
                    }
                }
            }
        }

        stage('Upload Report') {
            steps {
                script {
                    echo "Gitleaks exit code: ${security_check}"
                    withCredentials([string(credentialsId: 'defectdojo', variable: 'DEFECTDOJO_API_TOKEN')]) {
                            sh(script: '''
                            curl -X POST "${DEFECTDOJO_URL}/api/v2/import-scan/" \
                            -H "Authorization: Token $DEFECTDOJO_API_TOKEN" \
                            -H "Content-Type: multipart/form-data" \
                            -F "file=@gitleaks-report.json" \
                            -F "scan_type=Gitleaks Scan" \
                            -F "active=true" \
                            -F "engagement=${ENGAGEMENT_ID}"
                        ''')
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