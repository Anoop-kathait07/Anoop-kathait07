pipeline {
    agent any
    
    stages {
        stage('First Stage Build') {
            steps {
                script {
                    // This is the build stage; you can put your build commands here
                    sh 'echo "Hello world, this is a test project"'
                }
            }
            post {
                success {
                    // Notify on build success
                    emailext(
                        subject: "Regarding Jenkins build",
                        body: "#${BUILD_NUMBER} of ${JOB_NAME} has succeeded."
                    )
                }
            }
        }
        
        stage('Manual Approval') {
            when {
                expression {
                    currentBuild.resultIsBetterOrEqualTo('SUCCESS')
                }
            }
            steps {
                script {
                    // Send an email notification requesting approval
                    emailext(
                        subject: "Approval Required: Build #${BUILD_NUMBER} - ${JOB_NAME}",
                        body: "Hi Sir, I need your approval for this request."
                    )
                    // Wait for the approval
                    input "ProceedApproval"
                }
            }
        }

        stage('Deploy Success') {
            when {
                expression {
                    currentBuild.resultIsBetterOrEqualTo('SUCCESS')
                }
            }
            steps {
                script {
                    // Deployment steps for success go here
                    sh 'echo "Deployment for success: This is a test project"'
                }
            }
            post {
                success {
                    // Notify on deployment success
                    emailext(
                        subject: "Regarding Jenkins build",
                        body: "#${BUILD_NUMBER} of ${JOB_NAME} has deployed successfully."
                        recipient: "thakuranoop54321@gmail.com"  // Replace with the actual recipient's email address
                    )
                }
            }
        }

        stage('Deploy Failure') {
            when {
                expression {
                    currentBuild.resultIsBetterOrEqualTo('FAILURE')
                }
            }
            steps {
                script {
                    // Deployment steps for failure go here
                    sh 'echo "Deployment for failure: This is a test project"'
                }
            }
            post {
                failure {
                    // Notify on deployment failure
                    emailext(
                        subject: "Regarding Jenkins build",
                        body: "#${BUILD_NUMBER} of ${JOB_NAME} has failed to deploy."
                        recipient: "thakuranoop54321@gmail.com"  // Replace with the actual recipient's email address
                    )
                }
            }
        }
    }
}
