#!groovy


@Library('hv') _

def database_password = ""

def stageGatherCredentialsAndConfiguration() {
    stage('Gather Credentials and Configuration') {
        withCredentials([
            string(credentialsId: 'POWERDNS_PASS', variable: 'POWERDNS_PASS'),
        ]) {
            database_password = "${POWERDNS_PASS}"
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
        hv.stageDeployToMarathon(customImage, "", [[name: 'POWERDNS_PASS', value: "$database_password"]] )
    } catch (err) {
        hv.catchTopLevelError(err)
    } finally {
        hv.topLevelFinally()
    }
}

