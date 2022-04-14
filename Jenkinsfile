pipeline {
    agent any

    tools{
        maven 'local maven'
    }

    parameters{
        string(name: 'tomcat_dev', defaultValue: '18.237.67.245', description: 'Staging Server')
        string(name: 'tomcat_prod', defaultValue: '34.212.140.10', description: 'Production Server')
    }

    triggers {
        pollSCM('* * * * *')
    }

    stages {

        stage('Build') {
            steps {
                sh 'mvn clean package'
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
                        bat "scp -i C:/Users/chifu/Documents/Aws/tomcat.pem **/target/*.war ec2-user@${params.tomcat_dev}:/opt/tomcat/webapps"
                    }
                }
                stage ('Deploy to Production') {
                    steps {
                        bat "scp -i C:/Users/chifu/Documents/Aws/tomcat.pem **/target/*.war ec2-user@${params.tomcat_prod}:/opt/tomcat/webapps"
                    }
                }
            }
        }

    }

  
}