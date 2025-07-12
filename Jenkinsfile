pipeline {
    agent any

    environment {
        SLACK_CHANNEL = "jenkins-ci-cd"
    }

    stages {
        stage('Example') {
            steps {
                script {
                    AUTHOR_NAME = sh (
                        script: "git show -s --format='%an' HEAD",
                        returnStdout: true
                        ).trim()

                    slackSend channel: "${SLACK_CHANNEL}", message: "Build triggered by ${AUTHOR_NAME}"
                }
            }
        }
    }
}