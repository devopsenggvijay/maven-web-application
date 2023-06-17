pipeline{
          agent any
          tools {
          maven 'maven3.9.0'
          }
          

             stages{
                 stage('checkout Stage'){
                 steps{
                     sendSlackNotifications('STARTED')
                   git branch: 'development', credentialsId: 'f26ff227-0bcf-4fa5-856e-9d06e2538103', url: 'https://github.com/devopsenggvijay/maven-web-application.git'
  
                 }
             }
                 stage('Build') {
                  steps{
                      sh "mvn clean package" 
                  }
              } 
              
              stage('ExecuteSonarQube'){
                  steps{
                      sh "mvn sonar:sonar"
                  }
              }
              stage('DeployIntoNexus'){
                  steps{
                      sh "mvn clean deploy"
                  }
              }
              stage('DeployToTomcat'){
                  steps{
                      sshagent(['f0eb4938-1968-477e-9ab3-b0450a0a98fd']) {
    sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@13.48.27.216:/opt/apache-tomcat-9.0.75/webapps/"

}
                      
                  }
              }
        }
                post {
  success {
   sendSlackNotifications(currentBuild.result)
  }
  failure {
    sendSlackNotifications(currentBuild.result)
  }
}
  
}
        
           def sendSlackNotifications(String buildStatus = 'STARTED') {
  // build status of null means successful
  buildStatus =  buildStatus ?: 'SUCCESS'

  // Default values
  def colorName = 'RED'
  def colorCode = '#FF0000'
  def subject = "${buildStatus}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'"
  def summary = "${subject} (${env.BUILD_URL})"

// Override default values based on build status
  if (buildStatus == 'STARTED') {
    colorName = 'YELLOW'
    colorCode = '#FFFF00'
  } else if (buildStatus == 'SUCCESS') {
    colorName = 'GREEN'
    colorCode = '#00FF00'
  } else {
    colorName = 'RED'
    colorCode = '#FF0000'
  }

  // Send notifications
  slackSend (color: colorCode, message: summary, channel: '#citibank')
}

