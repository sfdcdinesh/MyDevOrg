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
}
        stage("Checkout source") {
            steps {
                checkout scm
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
                    stage('Deploye Code') {
            if (isUnix()) {
                rc = sh returnStatus: true, script: "${toolbelt} force:auth:jwt:grant --clientid ${CONNECTED_APP_CONSUMER_KEY} --username ${HUB_ORG} --jwtkeyfile ${jwt_key_file} --setdefaultdevhubusername --instanceurl ${SFDC_HOST}"
            }else{
                 rc = sh returnStatus: true, script: "\"${toolbelt}\" force:auth:jwt:grant --clientid ${CONNECTED_APP_CONSUMER_KEY} --username ${HUB_ORG} --jwtkeyfile \"${jwt_key_file}\" --setdefaultdevhubusername --instanceurl ${SFDC_HOST}"
            }
            if (rc != 0) { error 'hub org authorization failed' }

			println rc
                    }
                    
                }
            }
             } 
	}
