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
                bat 'docker build -t tomcatwebapp:1 .'
            }
        }    
    }

  
}