pipeline{
    agent any

    environment{
        PATH="opt/maven/bin:$PATH"
    }
    stages{
        stage("Git Checkout"){
            steps{
                git credentialsId: 'jenkinfile', url: 'https://github.com/Dasarivenkatesh/myweb.git'
            }
        }
        stage("maven build"){
            steps{
                sh "mvn clean package"
                sh "mv target/*.war  target/myweb.war"
            }
        }
        stage("tomca-deploy"){
            steps{
        
            sshagent(['tomcat']) {
            sh """
               scp -o StrictHostKeychecking =no target/myweb.war ec2-user@10.1.1.177:opt/tomcat9/webapps/
               ssh ec2-user@10.1.1.177 opt/tomcat/bin/shutdown.sh
               ssh ec2-user@10.1.1.177 opt/tomcat/bin/startup.sh
                // some block
                """
              }
            }  
        }
            
    }
}
