#!groovy


@Library('hv') _

def stageGatherCredentialsAndConfiguration() {
    stage('Gather Credentials and Configuration') {
    }
}

node {
    try {
        hv.stageCheckout()
        hv.stageSetupEnvironment()
        stageGatherCredentialsAndConfiguration()
        master_args = "--no-cache " +
            "--build-arg PROJECT_ID=${PROJECT_ID} " +
            "docker/"
        other_args = "--pull " +
            "--build-arg PROJECT_ID=${PROJECT_ID} " +
            "docker/"
        def customImage = hv.stageBuildContainer(master_args, other_args)
        hv.stageDeployToMarathon(customImage, testUrl)
    } catch (err) {
        hv.catchTopLevelError(err)
    } finally {
        hv.topLevelFinally()
    }
}

