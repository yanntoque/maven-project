pipeline{
    agent any
    /*
    The agent section specifies where the entire Pipeline, or a specific stage, will execute 
    in the Jenkins environment depending on where the agent section is placed. 
    The section must be defined at the top-level inside the pipeline block, but stage-level usage is optional.
    */
    
    
    /*
    The parameters directive provides a list of parameters which a user should provide when triggering the Pipeline.
    The values for these user-specified parameters are made available to Pipeline steps via the params object, see the Example for its specific usage.
    */ 
    parameters{
        string(name: 'tomcat_dev', defaultValue:'35.180.152.27', description:'Staging Server')
        string(name: 'tomcat_prod', defaultValue:'52.47.180.100', description:'Production Server')
    }

    /*
    The triggers directive defines the automated ways in which the Pipeline should be re-triggered.
    For Pipelines which are integrated with a source such as GitHub or BitBucket, triggers may not be necessary as webhooks-based integration will likely already be present.
    The triggers currently available are cron, pollSCM and upstream
    pollSCM : * * * * * == Jekins will poll our GitHub Repo looking for any changes and that every minute of every day of every week of every month
    This cron value is not the best it's just for demo purpose
    This will poll every branch and create a job where it finds a Jenkinsfile in the location and name you specify in the Multibranch Pipeline job configuration.
    */
    triggers{
        pollSCM('* * * * *')
    }    

    /*
    Added -T option to avoid Pseudo-terminal error
    */
    stages{
        stage('Initiate jenkins connection to EC2 instances'){
            parallel{
                    stage('Check connection from jenkins to ec2 instance 1'){
                        steps{
                            sh "ssh -T -i /var/lib/jenkins/tomcat-demo.pem ec2-user@${params.tomcat_dev}"
                        }
                        post {
                            success {
                                echo 'Connection successful'
                            }

                            failure {
                                echo 'Can\'t connect to EC2'
                            }
                        }
                    }

                    stage('Check connection from jenkins to ec2 instance 2'){
                        steps{
                            sh "ssh -T -i /var/lib/jenkins/tomcat-demo.pem ec2-user@${params.tomcat_prod}"
                        }
                        post {
                            success {
                                echo 'Connection successful'
                            }

                            failure {
                                echo 'Can\'t connect to EC2'
                            }
                        }
                }
            }
        }

        stage('Build'){
            steps{
                sh 'mvn clean package'
            }
            post{
                success{
                    echo 'Now archiving artifacts ...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
        

        /*
        Note : in order to make the deploy jobs working do not forget
        to change the owner of to the *.pem file to jenkins:jenkins
        */
        stage('Deploy to Staging'){
            steps{
                sh "scp -i /var/lib/jenkins/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
            }
        }
        
        stage('Deploy to PRODUCTION'){
            steps{
                timeout(time:5, unit:'DAYS'){
                    input message:'Approve PRODUCTION Deployment?'
                }
                
                sh "scp -i /var/lib/jenkins/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
            }
            post {
                success {
                    echo 'Code deployed to Production.'
                }

                failure {
                    echo ' Deployment to production server has failed.'
                }
            }
        }
    }
}