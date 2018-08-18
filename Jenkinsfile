pipeline {
    agent any
    String my_tag = env.GIT_COMMIT
    my_tag = my_tag.substring(0,6)

    stages{
        stage('Build'){
            steps{
                bat 'mvn clean package'
                bat "docker build . -t tomcatwebapp:${env.BUILD_ID}"
                bat "echo ${my_tag}"
            }
        }
    }
}