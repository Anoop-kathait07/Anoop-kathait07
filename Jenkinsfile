pipeline {
    agent any

    stages {
        stage('Send notification on Gmail') {
            steps {
                
                script {
                    'echo "Hello world this is a test mail"'
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
                    contentType: 'text/html'
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
                contentType: 'text/html'
            )
        }
    }
}

           
