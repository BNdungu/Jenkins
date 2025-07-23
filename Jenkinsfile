pipeline {
    agent any

    environment {
        SLACK_CHANNEL = "jenkins-ci-cd"
    }

    stages {
        stage('Notify with Git Info') {
            steps {
                script {
                    def (AUTHOR_NAME, AUTHOR_EMAIL, COMMIT_DNT) = sh(
                        script: "git show -s --format='%an,%ae,%ad' HEAD",
                        returnStdout: true
                    ).trim().tokenize(',')

                    def BRANCH_NAME = sh(
                        script: "git rev-parse --abbrev-ref HEAD",
                        returnStdout: true
                    ).trim()

                    withCredentials([file(credentialsId: 'slack-user-map', variable: 'SLACK_USER_MAP')]) {
                        def emailToSlackID = readJSON file: "${SLACK_USER_MAP}"
                        def slackID = emailToSlackID.get(AUTHOR_EMAIL, '')

                        slackSend(
                            channel: "${env.SLACK_CHANNEL}",
                            message: """
:white_check_mark: *Build Success!*

Hey team, the Jenkins job `${env.JOB_NAME}` just completed build #${env.BUILD_NUMBER} successfully. This run was triggered from the `${BRANCH_NAME}` branch. The latest commit was made by *${AUTHOR_NAME}* <@${slackID}> on *${COMMIT_DNT}*. You can review the full build logs <${env.BUILD_URL}console|here>.
"""
                        )
                    }
                }
            }
        }

        post {
    failure {
        script {
            def (AUTHOR_NAME, AUTHOR_EMAIL, COMMIT_DNT) = sh(
                script: "git show -s --format='%an,%ae,%ad' HEAD",
                returnStdout: true
            ).trim().tokenize(',')

            def BRANCH_NAME = sh(
                script: "git rev-parse --abbrev-ref HEAD",
                returnStdout: true
            ).trim()

            withCredentials([file(credentialsId: 'slack-user-map', variable: 'SLACK_USER_MAP')]) {
                def emailToSlackID = readJSON file: "${SLACK_USER_MAP}"
                def slackID = emailToSlackID.get(AUTHOR_EMAIL, '')

                slackSend(
                    channel: "${env.SLACK_CHANNEL}",
                    message: """
:x: *Build Failed!*

Heads up! The Jenkins job `${env.JOB_NAME}` failed on build #${env.BUILD_NUMBER}. This was triggered from the `${BRANCH_NAME}` branch. The last commit was by *${AUTHOR_NAME}* <@${slackID}> on *${COMMIT_DNT}*. Check the console output for more details: <${env.BUILD_URL}console|View Logs>.
"""
                )
            }
        }
    }
}

    }
}