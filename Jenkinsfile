pipeline {
    agent any
    stages{
        stage('Build maven-project'){
            steps {
                sh 'mvn clean package'
            }
        }

        stage('check user & test simple command from docker'){
            steps{
                sh 'echo $USER'
                sh 'docker image ls'
            }
        }

        stage('Docker image build : webappbuildfromjenkins'){
            steps{
                /* BUILD_ID : 
                Every time we hit it build on jenkins,this number will increment.
                It's used as a version number for the generated docker image.
                That way the image will be tagged with the version number.
                */
                sh "docker build . -t webappbuildfromjenkins:latest -t webappbuildfromjenkins:${env.BUILD_ID}"
            }
            post {
                    success {
                        echo 'Image has been successufully built'
                    }

                    failure {
                        echo 'Image build failed.'
                }
            }
        }
        // stage('Run webappbuildfromjenkins'){
        //     steps{
        //         /* 
        //         This will run a container based on the image that has been
        //         */
        //         sh "docker run -d -p 8181:8080  webappbuildfromjenkins:latest"
        //     }
        // }
    }
}