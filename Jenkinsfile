#!groovy
import groovy.json.JsonSlurperClassic
node {

    def BUILD_NUMBER=env.BUILD_NUMBER
    def RUN_ARTIFACT_DIR="tests/${BUILD_NUMBER}"
    def SFDC_USERNAME

    def HUB_ORG=env.HUB_ORG_DH
    def SFDC_HOST = env.SFDC_HOST_DH
    def JWT_KEY_CRED_ID = env.JWT_CRED_ID_DH
    def CONNECTED_APP_CONSUMER_KEY=env.CONNECTED_APP_CONSUMER_KEY_DH

    println 'KEY IS' 
    println JWT_KEY_CRED_ID
    println HUB_ORG
    println SFDC_HOST
    println CONNECTED_APP_CONSUMER_KEY
    def toolbelt = tool 'toolbelt'

    stage('checkout source') {
        // when running in multi-branch job, one must issue this command
        checkout scm
    }

    withCredentials([file(credentialsId: JWT_KEY_CRED_ID, variable: 'JWT_KEY_FILE')]) {

        stage('Install CLI') {
                rc = command "npm install -g sfdx-cli --loglevel verbose"
                println(rc)
                if (rc != 0) {
                    error 'Salesforce CLI not installed properly.'
                }
    script {
        def command = "sfdx force:auth:jwt:grant --clientid ${CONNECTED_APP_CONSUMER_KEY} --jwtkeyfile ${JWT_KEY_FILE} --username ${HUB_ORG} --setdefaultdevhubusername --instanceurl ${SFDC_HOST}"
        if (isUnix()) {
            sh command
        } else {
            bat command
        }
    }
}
    }

}