pipeline {
    agent any
    
        stages {
            stage ('first stage build') {
                steps {
                    sh 'echo "Hello word this is a test project"'
                }
            }
            stage ('Manual Aproval') {
                When{
                    expersion {
                        currentBuild.resultIsBatterorEqualTo('SUCCESS')
                    }
                }
                steps
                // Send an email notification request approval
                emailext(
                    subject: 'Approval Required: Build #${BUILD_NUMBER} - ${JOB_NAME}',
                    Body: Hii Sir, Need your approval for this reqest
                    )
                // Wait for the approval
                 input 'ProceedApproval'
            }
        }

    Stage ('Deploy') {
        when {
            expersion {
                currentBuild.resultIsBatterorEqualTo('SUCCESS')
            }
        }
        steps
        // Deployment is here
        sh 'echo "Hello world this is a test project"'
    }


Post {
    SUCCESS
    // Notify on build SUCCESS
    emailext(
                    subject: 'Approval Required: Build #${BUILD_NUMBER} - ${JOB_NAME}',
                    Body: Hii Sir, Need your approval for this reqest
                    )
}
}


    
        
    
        
                    
                
    

