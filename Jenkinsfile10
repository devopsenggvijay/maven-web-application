node{
       def mavenHome = tool name:'maven3.9.0'
       stage('Checkout Code'){
        git branch: 'development', credentialsId: 'f26ff227-0bcf-4fa5-856e-9d06e2538103', url: 'https://github.com/devopsenggvijay/maven-web-application.git'
       }
     stage('Build'){
        sh "${mavenHome}/bin/mvn clean package"
       }
       stage('ExecuteSonarQubeReport'){
           sh "${mavenHome}/bin/mvn clean sonar:sonar"
       }
       stage('UploadArtifactsintoNexus'){
           sh "${mavenHome}/bin/mvn clean deploy"
       }
       stage('DeployIntoTomcat'){
        sshagent(['f0eb4938-1968-477e-9ab3-b0450a0a98fd']) {
          sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@13.48.56.141:/opt/apache-tomcat-9.0.75/webapps/"
 
           } 
         }
    }
