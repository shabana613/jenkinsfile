pipeline{
    agent any
    stages{
        stage('SCM'){
            steps{
                //git commit/pull
                git credentialsId: 'github', url: 'https://github.com/javahometech/my-app'
            }
        }
        
        stage('MVN Package'){
            steps{
                sh 'mvn clean package'
            }
        }
        
        stage('Tomcat Deploy'){
            steps{
                sh 'mv target/myweb*.war target/myweb.war'
                sshagent(credentials: ['slave-two-linux'], ignoreMissing: true) {
                // some block
                    sh "scp -o StrictHostKeyChecking=no target/myweb*.war ec2-user@172.31.32.133:/opt/tomcat8/webapps/myweb.war"
                    sh "ssh ec2-user@172.31.32.133 /opt/tomcat8/bin/startup.sh"
                    sh "ssh ec2-user@172.31.32.133 /opt/tomcat8/bin/shutdow.sh"       

                  }

            }
        }
    }
}
