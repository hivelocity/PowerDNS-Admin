#!groovy


@Library('hv') _

def database_password = "";

def stageGatherCredentialsAndConfiguration() {
    stage('Gather Credentials and Configuration') {
        withCredentials([
            string(credentialsId: 'POWERDNS_PASS', variable: 'POWERDNS_PASS'),
        ]) {
            sh "echo 'SQLA_DB_PASSWORD = \"${POWERDNS_PASS}\"' >> powerdnsadmin/default_config.py"
            sh "echo 'SQLALCHEMY_DATABASE_URI = \"mysql://\"+SQLA_DB_USER+\":${POWERDNS_PASS}@\"+SQLA_DB_HOST+\"/\"+SQLA_DB_NAME' >> powerdnsadmin/default_config.py"
        }
    }
}

node {
    try {
        hv.stageCheckout()
        hv.stageSetupEnvironment()
        stageGatherCredentialsAndConfiguration()
        master_args = "--no-cache " +
            "--build-arg PROJECT_ID=${PROJECT_ID} " +
            "."
        other_args = "--pull " +
            "--build-arg PROJECT_ID=${PROJECT_ID} " +
            "."
        def customImage = hv.stageBuildContainer(master_args, other_args)
        hv.stageDeployToMarathon(customImage, "")
    } catch (err) {
        hv.catchTopLevelError(err)
    } finally {
        hv.topLevelFinally()
    }
}

