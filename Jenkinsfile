pipeline{
    agent any
    
    environment{
        PATH = "/opt/maven3/bin:$PATH"
    }
    stages{
        stage("Git Checkout"){
            steps{
                git credentialsId: 'javahome', url: 'https://github.com/bhuvisha10/myproject.git'
            }
        }
        stage("Maven Build"){
            steps{
                sh "mvn clean package"
                sh "mv target/*.war target/myweb.war"
            }
        }
        stage("deploy-dev"){
            steps{
                sshagent(['tomcat-new']) {
                sh """
                    scp -o StrictHostKeyChecking=no target/myweb.war  ec2-user@172.31.37.193:/home/ec2-user/apache-tomcat-9.0.91/webapps/
                    
                    ssh ec2-user@172.31.37.193 /home/ec2-user/apache-tomcat-9.0.91/bin/shutdown.sh
                    
                    ssh ec2-user@172.31.37.193 /home/ec2-user/apache-tomcat-9.0.91/bin/startup.sh
                
                """
            }
            
            }
        }
