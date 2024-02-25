pipeline {
    agent any

    environment {
        // Define your environment variables here
        HUB_ORG_DH = 'bhagirath.mali@1pwc.com'
        SFDC_HOST_DH = 'https://pwc-7cc-dev-ed.develop.my.salesforce.com'
        CONNECTED_APP_CONSUMER_KEY_DH = '3MVG95mg0lk4bathN3NYEW7Q4ydg2C3AZwQNN8P50c8s5fApgiYi3xtRmtSmo.KqtJOfWg1h6G5VqYzDPv1CG'
        // The ID for your JWT credentials stored in Jenkins
        JWT_CRED_ID_DH = '47792772-f37d-4d8a-b435-d5956d70b4b6'
    }

    stages {
        stage('Authenticate with Salesforce Dev Hub') {
            steps {
                script {
                    // Use withCredentials to securely inject your JWT key file into the environment
                    withCredentials([file(credentialsId: env.JWT_CRED_ID_DH, variable: 'JWT_KEY_FILE')]) {
                        // Determine script execution command based on OS
                        def authCommand = isUnix() ?
                            "sfdx force:auth:jwt:grant --clientid ${env.CONNECTED_APP_CONSUMER_KEY_DH} --jwtkeyfile ${JWT_KEY_FILE} --username ${env.HUB_ORG_DH} --setdefaultdevhubusername --instanceurl ${env.SFDC_HOST_DH}" :
                            "sfdx force:auth:jwt:grant --clientid ${env.CONNECTED_APP_CONSUMER_KEY_DH} --jwtkeyfile ${JWT_KEY_FILE} --username ${env.HUB_ORG_DH} --setdefaultdevhubusername --instanceurl ${env.SFDC_HOST_DH}"

                        // Execute authentication command
                        if (isUnix()) {
                            sh script: authCommand, returnStatus: true
                        } else {
                            bat script: authCommand, returnStatus: true
                        }
                    }
                }
            }
        }

        // Define additional stages as needed
    }
}
