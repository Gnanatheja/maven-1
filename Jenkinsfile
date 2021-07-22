node('master') 
{
    stage('ContinuousDownload')
    {
        git 'https://github.com/intelliqittrainings/maven.git'             
    }
    stage('ContinuousBuild')
    {
        sh 'mvn package'
    }
    stage('Deploy to Tomcat'){
      sshagent(['Tomcat-cred']) {
         sh """
           scp -o StrictHostKeyChecking=no target/*.war ubuntu@172.31.2.175:/home/ubuntu
           ssh -o StrictHostKeyChecking=no ubuntu@172.31.2.175 'cp -r /home/ubuntu/*.war /opt/tomcat/latest/webapps/'
         """
      }
    stage('ContinuousTesting')
    {
        git 'https://github.com/intelliqittrainings/FunctionalTesting.git'
        sh 'java -jar /home/ubuntu/.jenkins/workspace/ScriptedPipeline/testing.jar'
    }
    stage('ContinuousDelivery')
    {
    
    sh 'scp /home/ubuntu/.jenkins/workspace/ScriptedPipeline/webapp/target/webapp.war ubuntu@172.31.10.233:/var/lib/tomcat9/webapps/prodapp.war'}
}
