pipeline {
    agent any
    stages{
        stage('Build'){
            steps{
                String my_tag = env.GIT_COMMIT
                my_tag = my_tag.substring(0,6)
                bat 'mvn clean package'
                bat "docker build . -t tomcatwebapp:${env.BUILD_ID}"
                bat "echo ${my_tag}"
            }
        }
    }
}