pipeline {
    agent any
    stages{
        stage('Build'){
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
        stage('Deploy to Staging'){
            steps{
                build job: 'deploy-to-staging'
            }
        }
        stage('Deploy to Prod'){
            steps{
                /*timeout build will fail in case the timeout is reached*/
                timeout(time:5, unit:'DAYS'){
                    input message:'Approve Production Deployment ?' /*, submitter:jenkins user allowed to approve production deployment*/
                }
                build job: 'deploy-to-prod'
            }
            post{
                success{
                    echo 'The code has been to PRODUCTION'
                    /*Possibility to send email to the person that triggered the job*/
                }
                failure{
                    echo 'Deployment failed'
                }
            }
        }
    }
}