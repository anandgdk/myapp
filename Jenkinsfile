pipeline {
    agent any
    options {
     buildDiscarder logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '15', numToKeepStr: '5')
    }
    parameters {
    string defaultValue: 'develop', description: 'choose develop branch', name: 'branch name'
    }
    stages {
        stage('Git Checkout') {
            steps {
                git credentialsId: 'Git-hub', url: 'https://github.com/anandgdk/myapp'
            }
        }
        
        stage('Maven Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        
        stage('Tomcat-dev') {
            steps {
                sshagent(['ec2-user']) {
                    //copy war file
                
                sh 'scp -o StrictHostKeyChecking=no target/*.war ec2-user@172.31.2.64:/opt/tomcat8/webapps/app.war'
                //stop tomcat
                sh "ssh ec2-user@172.31.2.64 /opt/tomcat8/bin/shutdown.sh"
                //start tomcat
                sh "ssh ec2-user@172.31.2.64 /opt/tomcat8/bin/startup.sh"
                
                 }
            }
        }
    }
}
