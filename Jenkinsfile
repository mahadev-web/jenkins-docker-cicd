pipeline {
    agent any

    stages {
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t flask-app:v1 .'
            }
        }
    }

    post {
        success {
            echo 'Docker image built successfully'
        }

        failure {
            withCredentials([string(credentialsId: 'slack-webhook', variable: 'SLACK_WEBHOOK')]) {
                sh '''
                curl -X POST \
                -H "Content-type: application/json" \
                --data "{\"text\":\"🚨 Jenkins Build FAILED: ${JOB_NAME} #${BUILD_NUMBER}\"}" \
                $SLACK_WEBHOOK
                '''
            }
        }
    }
}
