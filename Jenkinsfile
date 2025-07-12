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


                    slackSend(
                        channel: "${env.SLACK_CHANNEL}",
                        message: """
                        :white_check_mark: *Build Success!*
                        *• Job:* `${env.JOB_NAME}`
                        *• Build:* #${env.BUILD_NUMBER}
                        *• Branch:* `${env.BRANCH_NAME}`
                        *• Commit:* `${shortCommit}`
                        *• Author:* ${AUTHOR_NAME} <@${env.COMMITTER_SLACK_ID}>
                        *• Time:* ${COMMIT_DNT}
                        *• Console:* <${env.BUILD_URL}console|View Logs>
                        """
                    )

                }
            }
        }
    }
}