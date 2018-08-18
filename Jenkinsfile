pipeline {
    agent any
    
    //my_tag = my_tag.substring(0,6)

    stages{
        stage('Build'){
            steps{
                def my_tag = env.GIT_COMMIT
                bat 'mvn clean package'
                bat "docker build . -t tomcatwebapp:${env.BUILD_ID}"
                bat "echo ${my_tag}"
            }
        }
    }
}