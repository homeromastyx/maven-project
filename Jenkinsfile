pipeline {
    agent any
    stages {
        stage('Build'){
            steps{
                sh '/var/jenkins_home/tools/hudson.tasks.Maven_MavenInstallation/Maven/bin/mvn clean package'
                sh 'docker build . -t tomcatwebapp:${env.BUILD_ID}'
            }
        }
    }
}