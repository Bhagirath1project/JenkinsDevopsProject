pipeline {
    agent any

    environment {
        // Define your environment variables here
        SFDC_HOST = "${env.SFDC_HOST_DH}"
        JWT_KEY_CRED_ID = "${env.JWT_CRED_ID_DH}"
        CONNECTED_APP_CONSUMER_KEY = "${env.CONNECTED_APP_CONSUMER_KEY_DH}"
        HUB_ORG = "${env.HUB_ORG_DH}"
    }

    stages {
        stage('Checkout Source') {
            steps {
                checkout scm
            }
        }

        stage('Authenticate with Salesforce') {
            steps {
                withCredentials([file(credentialsId: "${JWT_KEY_CRED_ID}", variable: 'JWT_KEY_FILE')]) {
                    script {
                        // Use 'bat' for Windows nodes or 'sh' for Unix/Linux nodes
                        if (isUnix()) {
                            sh """
                            sfdx force:auth:jwt:grant --clientid ${CONNECTED_APP_CONSUMER_KEY} --jwtkeyfile ${JWT_KEY_FILE} --username ${HUB_ORG} --setdefaultdevhubusername --instanceurl ${SFDC_HOST}
                            """
                        } else {
                            bat """
                            sfdx force:auth:jwt:grant --clientid ${CONNECTED_APP_CONSUMER_KEY} --jwtkeyfile ${JWT_KEY_FILE} --username ${HUB_ORG} --setdefaultdevhubusername --instanceurl ${SFDC_HOST}
                            """
                        }
                    }
                }
            }
        }

        stage('Deploy Code') {
            steps {
                script {
                    // Deployment command example
                    // Adjust according to your specific deployment process
                    if (isUnix()) {
                        sh "sfdx force:source:deploy -x path/to/your/package.xml -u ${HUB_ORG}"
                    } else {
                        bat "sfdx force:source:deploy -x path/to/your/package.xml -u ${HUB_ORG}"
                    }
                }
            }
        }
    }

    post {
    always {
        // Example step
        echo 'This will always run'
    }
}
}
