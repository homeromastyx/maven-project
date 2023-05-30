pipeline{
    agent any

    parameters {
        string(name: 'tomcat_staging', defaultValue: '52.207.254.247', description: 'Staging server')
        string(name: 'tomcat_production', defaultValue: '54.159.183.41', description: 'Production server')
    }

    triggers {
        pollSCM('* * * * *')
    }

    stages {
        stage('Build'){
            steps {
                sh '/var/jenkins_home/tools/hudson.tasks.Maven_MavenInstallation/Maven/bin/mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage('Deployments'){
            parallel{
                stage('Deploy to Staging'){
                    steps{
                        sh "sudo scp -o StrictHostKeyChecking=no -i /var/jenkins_home/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_staging}:/var/lib/tomcat9/webapps"
                    }
                }

                stage('Deploy to production'){
                    steps{
                        sh "sudo scp -o StrictHostKeyChecking=no -i /var/jenkins_home/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_production}:/var/lib/tomcat9/webapps"
                    }
                }
            }
        }
    }
}