pipeline {
    agent any

    stages {
        stage('create a directory') {
            steps {
                sh '''
                mkdir money
                rm -rf money
                '''
            }
        }
        stage('creating a file') {
            steps {
                sh '''
                touch money.txt
                rm -rf money.txt
                '''
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    } 
    post {
       success {
           slackSend color: '#2EB67D',
           channel: 'charlsee-revive', 
           message: "REVIVE Project Build Status" +
           "\n Project Name: REVIVE" +
           "\n Job Name: ${env.JOB_NAME}" +
           "\n Build number: ${currentBuild.displayName}" +
           "\n Build Status : SUCCESS" +
           "\n Build url : ${env.BUILD_URL}"
       }
       failure {
           slackSend color: '#E01E5A',
           channel: 'charlsee-revive',  
           message: "revive Project Build Status" +
           "\n Project Name: REVIVE" +
           "\n Job Name: ${env.JOB_NAME}" +
           "\n Build number: ${currentBuild.displayName}" +
           "\n Build Status : FAILED" +
           "\n Build User : Tia" +
           "\n Action : Please check the console output to fix this job IMMEDIATELY" +
           "\n Build url : ${env.BUILD_URL}"
       }
       unstable {
           slackSend color: '#ECB22E',
           channel: 'charlsee-revive', 
           message: "revive Project Build Status" +
           "\n Project Name: REVIVE" +
           "\n Job Name: ${env.JOB_NAME}" +
           "\n Build number: ${currentBuild.displayName}" +
           "\n Build Status : UNSTABLE" +
           "\n Action : Please check the console output to fix this job IMMEDIATELY" +
           "\n Build url : ${env.BUILD_URL}"
       }   
   }
}    