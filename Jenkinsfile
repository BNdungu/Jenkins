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
    *• Job:* `${env.JOB_NAME}`
    *• Build:* #${env.BUILD_NUMBER}
    *• Branch:* `${BRANCH_NAME}`
    *• Committer:* ${AUTHOR_NAME} <@${slackID}>
    *• Commit Time:* ${COMMIT_DNT}
    *• Console:* <${env.BUILD_URL}console|View Logs>
"""
                        )
                    }
                }
            }
        }
    }
}