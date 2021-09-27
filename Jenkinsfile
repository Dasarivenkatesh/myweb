pipeline{
    agent any

    environment{
        PATH="opt/maven/bin:$PATH"
    }
    stages{
        stage("Git Checkout"){
            steps{
                git credentialsId: 'jenkinsfile', url: 'https://github.com/Dasarivenkatesh/myweb.git'
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
        
            sshagent(credentials: ['dev-new'], ignoreMissing: true) {

                  sh """
                     scp -o StrictHostKeyChecking=no target/myweb.war ec2-user@10.1.1.177:/opt/apache-tomcat-9.0.53/webapps

                     
                     // some block
                     """
                 }
            }  
        }
            
    }
}
