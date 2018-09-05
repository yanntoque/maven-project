pipeline {
    agent any

    environment{
        my_tag = "c:\temp"
        //my_tag = my_tag
    }
     
    

    stages{
        stage('Build'){
            steps{
                //bat 'mvn clean package'
                //bat "docker build . -t tomcatwebapp:${env.BUILD_ID}"
                bat "echo ${my_tag}"
            }

        }
    }
}