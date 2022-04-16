pipeline {
    agent any

    tools{
        maven 'local maven'
    }

    parameters{
        string(name: 'tomcat_dev', defaultValue: '54.203.186.17', description: 'Staging Server')
        string(name: 'tomcat_prod', defaultValue: '18.237.224.181', description: 'Production Server')
    }

    triggers {
        pollSCM('* * * * *')
    }

    stages {

        stage('Build') {
            steps {
                bat 'mvn clean package'
            }
            post {
                success {
                    echo 'Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }    

        stage ('Deployments') {
            parallel {
                stage ('Deploy to Staging') {
                    steps {
                        bat "scp -o StrictHostKeyChecking=no -i C:/Users/chifu/Documents/Tools/tomcat.pem **/target/*.war ec2-user@${params.tomcat_dev}:/home/ec2-user/tomcat/webapps"
                    }
                }
                stage ('Deploy to Production') {
                    steps {
                        bat "scp -o StrictHostKeyChecking=no -i C:/Users/chifu/Documents/Tools/tomcat.pem **/target/*.war ec2-user@${params.tomcat_prod}:/home/ec2-user/tomcat/webapps"
                    }
                }
            }
        }

    }

  
}