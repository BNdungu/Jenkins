pipeline {
    agent any

    environments{
        SLACK_CHANNEL = "jenkins-ci-cd"
    }

    stages {
        stage('Example') {
            steps {
                script {
                    def authorName = env.GIT_COMMIT_AUTHOR
                    slackSend channel: "${SLACK_CHANNEL}", message: "Build triggered by ${authorName}"
                }
            }
        }
    }
}