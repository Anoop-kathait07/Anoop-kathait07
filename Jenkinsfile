pipeline {
    agent any

    stages {
        stage('Send Notification on Gmail') {
            steps {
                // Checkout the code from GitHub
                script {
                    'echo "this is a test project"'
                }
            }
        }

        stage('Build') {
            steps {
                // Your build steps go here
                sh 'echo "Building..."'
            }
        }

        stage('Manual Approval') {
            when {
                expression {
                    currentBuild.resultIsBetterOrEqualTo('SUCCESS')
                }
            }
            steps {
                // Send an email notification requesting approval
                emailext (
                    subject: 'Approval Required: Build #${BUILD_NUMBER} - ${JOB_NAME}',
                    body: """<p>Build #${BUILD_NUMBER} of ${JOB_NAME} requires your approval to proceed.</p>
                             <p>Click <a href="${BUILD_URL}/input/ProceedApproval/build?delay=0sec">here</a> to approve.</p>""",
                    recipientProviders: [[$class: 'DevelopersRecipientProvider']],
                    mimeType: 'text/html'  // Use mimeType instead of contentType
                )
                // Wait for user input (manual approval)
                input 'ProceedApproval'
            }
        }

        stage('Deploy') {
            when {
                expression {
                    currentBuild.resultIsBetterOrEqualTo('SUCCESS')
                }
            }
            steps {
                // Your deployment steps go here
                sh 'echo "Deploying..."'
            }
        }
    }

    post {
        failure {
            // Notify on build failure
            emailext (
                subject: 'Build Failed: Build #${BUILD_NUMBER} - ${JOB_NAME}',
                body: """<p>Build #${BUILD_NUMBER} of ${JOB_NAME} has failed.</p>""",
                recipientProviders: [[$class: 'DevelopersRecipientProvider']],
                mimeType: 'text/html'  // Use mimeType instead of contentType
            )
        }
    }
}

           
