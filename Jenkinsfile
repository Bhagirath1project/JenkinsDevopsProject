pipeline {
    agent any

    environment {
        // Define your environment variables here
        HUB_ORG_DH = 'bhagirath.mali@1pwc.com'
        SFDC_HOST_DH = 'https://pwc-7cc-dev-ed.develop.my.salesforce.com' // Make sure to use the correct login URL
        CONNECTED_APP_CONSUMER_KEY_DH = '3MVG95mg0lk4bathN3NYEW7Q4ydg2C3AZwQNN8P50c8s5fApgiYi3xtRmtSmo.KqtJOfWg1h6G5VqYzDPv1CG'
        JWT_CRED_ID_DH = '47792772-f37d-4d8a-b435-d5956d70b4b6'
    }

    stages {
        stage('Authenticate with Salesforce Dev Hub') {
            steps {
                script {
                    withCredentials([file(credentialsId: env.JWT_CRED_ID_DH, variable: 'JWT_KEY_FILE')]) {
                        def authCommand = "sfdx force:auth:jwt:grant --clientid ${env.CONNECTED_APP_CONSUMER_KEY_DH} --jwtkeyfile ${JWT_KEY_FILE} --username ${env.HUB_ORG_DH} --setalias myhuborg --instanceurl ${env.SFDC_HOST_DH}"
                        if (isUnix()) {
                            sh script: authCommand, returnStatus: true
                        } else {
                            bat script: authCommand, returnStatus: true
                        }
                    }
                }
            }
        }
        stage('Check Salesforce CLI') {
    steps {
        script {
            if (isUnix()) {
                sh 'sfdx --version'
            } else {
                bat 'sfdx --version'
            }
        }
    }
}

        stage('Deploy to Salesforce') {
            steps {
                script {
                    // Assuming your Salesforce metadata is in a directory named 'force-app'
                    def deployCommand = "sfdx force:source:deploy -p force-app -u ${env.HUB_ORG_DH} --wait 10"
                    if (isUnix()) {
                        sh script: deployCommand, returnStatus: true
                    } else {
                        bat script: deployCommand, returnStatus: true
                    }
                }
            }
        }
    }
}
