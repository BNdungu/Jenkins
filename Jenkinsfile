pipeline {
    agent any

    environment {
        SLACK_CHANNEL = "jenkins-ci-cd"
    }

    stages {
        stage('Example') {
            steps {
                script {
                    def (AUTHOR_NAME, AUTHOR_EMAIL, COMMIT_DNT) = sh(
                        script: "git show -s --format='%an,%ae,%ad' HEAD",
                        returnStdout: true
                    ).trim().tokenize(',')


                    slackSend channel: "${SLACK_CHANNEL}", message: "Build triggered by ${AUTHOR_NAME} of email ${AUTHOR_EMAIL} at ${COMMIT_DNT}"
                }
            }
        }
    }
}