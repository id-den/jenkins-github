#!groovy
node {

    try {
        stage('Build') {
            notifyStarted()

            echo "Branch Name: ${env.BRANCH_NAME}"

        }
        stage('PR') {
            echo "Check PR"

            if (env.CHANGE_TITLE =~ /skip ci/) {
                echo "Change Title with [skip ci]: ${env.CHANGE_TITLE}"
            }
            else if (env.CHANGE_TITLE =~ /skip db/) {
                echo "Change Title with [skip db]: ${env.CHANGE_TITLE}"
            }
            else {
                echo "${env.CHANGE_TITLE}"
            }


            notifySuccessful()
        }

        stage('PR Approve') {
            echo "Check PR Approve"
            echo "Change Target: ${env.CHANGE_TARGET}"
            echo "Change URL: ${env.CHANGE_URL}"

            checkout([$class: 'GitSCM',
              branches: [[name: '${ghprbPullTitle}']],
              doGenerateSubmoduleConfigurations: false,
              extensions: [],
              submoduleCfg: [],
              userRemoteConfigs: [[refspec: '+refs/pull/*:refs/remotes/origin/pr/*',
              url: 'https://github.com/id-den/jenkins-github.git']]])

            if (currentBuild.result == 'SUCCESS') {
                echo "SUCCESS"
            }
            else {
                echo "Something went wrong!"
            }
        }


    } catch (e) {
        currentBuild.result = 'FAILED'
        notifyFailed()
        throw e
    }
}

// Slack
def notifyStarted() {
    slackSend (color: '#FFFF00', message: "STARTED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
}

def notifySuccessful() {
    slackSend (color: '#00FF00', message: "SUCCESSFUL: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
}

def notifyFailed() {
    slackSend (color: '#FF0000', message: "FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
}
