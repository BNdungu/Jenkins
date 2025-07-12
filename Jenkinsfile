pipeline {
    agent any

    environment {
        SLACK_CHANNEL = "jenkins-ci-cd"
        COMMITTER_SLACK_ID = "U094GJVT6FP"
    }

    stages {
        stage('Example') {
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

                    slackSend(
                        channel: "${SLACK_CHANNEL}",
                        message: """
                        :white_check_mark: *Build Success!*
                        *• Job:* `${env.JOB_NAME}`
                        *• Build:* #${env.BUILD_NUMBER}
                        *• Branch:* `${BRANCH_NAME}`
                        *• Author:* ${AUTHOR_NAME} <@${COMMITTER_SLACK_ID}>
                        *• Time:* ${COMMIT_DNT}
                        *• Console:* <${env.BUILD_URL}console|View Logs>
                        """
                    )

                }
            }
        }
    }
}