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
        stage("build"){
            steps{
                sh "mvn clean package"
                sh "mv target/*.war  target/myweb.war"
            }
        }
        stage("artifoctory-backup"){
            steps{
                nexusArtifactUploader artifacts: [
                    [artifactId: 'myweb', 
                     classifier: '', 
                     file: 'target/myweb-8.2.0.war', 
                     type: 'war']], 
                     credentialsId: 'nexus3', 
                     groupId: 'in.javahome', 
                     nexusUrl: '18.234.158.108:8081/', 
                     nexusVersion: 'nexus3', 
                     protocol: 'http',
                     repository: 'http://18.234.158.108:8081/repository/version-release/',
                     version: '8.2.0'
            }
        }
         stage("tomcat-deploy"){
            steps{
        
            sshagent(credentials: ['dev-new'], ignoreMissing: true) {

                  sh """
                     scp -o StrictHostKeyChecking=no target/myweb.war ec2-user@10.1.1.177:/opt/apache-tomcat-9.0.53/webapps
                     """
                 }
            }  
         }    
            
    }
}
