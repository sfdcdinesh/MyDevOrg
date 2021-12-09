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

        SFDX_HOME                      = "\sfdx\bin"
        SFDX_USE_GENERIC_UNIX_KEYCHAIN = true
        HUB_ORG                        = "dinesh.ghattamaneni@gmail.com.trainingorg"
        SFDC_HOST                      = "https://login.salesforce.com"
        JWT_KEY_CRED_ID                = "1c6f302f-7137-45b2-a14e-209e5791fbbb"
        JWT_KEY_FILE                   = "server.key"
        CONNECTED_APP_CONSUMER_KEY     = "3MVG9fe4g9fhX0E5ghqkkG3foTU5nG1QX.yWFy7kIio87StdbX6cc72Pyo2dlM2sqYAJ5XcpoPcDNB2.Yz4q."
    }
        stage("Checkout source") {
            steps {
                checkout scm
            }
        }
}
}
}
        stage("Run build") {
            steps {
                withCredentials([file(credentialsId: JWT_KEY_CRED_ID, variable: "JWT_KEY_FILE")]) {

                    script {
                        echo "on branch name: ${BRANCH}"
                        echo "1. DEV HUB auth"
                        def authStatus = sh returnStatus: true, script: "${SFDX_HOME}/sfdx force:auth:jwt:grant -i ${CONNECTED_APP_CONSUMER_KEY} -u ${HUB_ORG} -f ${JWT_KEY_FILE} -s -r ${SFDC_HOST} --json --loglevel debug"
                        if (authStatus != 0) {
                            error "DEV HUB authorization failed"
                        } else {
                            echo "Successfully authorized to DEV HUB ${HUB_ORG}"
                        }
