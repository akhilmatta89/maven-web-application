/*

pipeline{

agent any

tools{
maven 'maven3.8.2'

}

triggers{
pollSCM('* * * * *')
}

options{
timestamps()
buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5'))
}

stages{

  stage('CheckOutCode'){
    steps{
    git branch: 'development', credentialsId: '957b543e-6f77-4cef-9aec-82e9b0230975', url: 'https://github.com/devopstrainingblr/maven-web-application-1.git'
	
	}
  }
  
  stage('Build'){
  steps{
  sh  "mvn clean package"
  }
  }
/*
 stage('ExecuteSonarQubeReport'){
  steps{
  sh  "mvn clean sonar:sonar"
  }
  }
  
  stage('UploadArtifactsIntoNexus'){
  steps{
  sh  "mvn clean deploy"
  }
  }
  
  stage('DeployAppIntoTomcat'){
  steps{
  sshagent(['bfe1b3c1-c29b-4a4d-b97a-c068b7748cd0']) {
   sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@35.154.190.162:/opt/apache-tomcat-9.0.50/webapps/"    
  }
  }
  }

}//Stages Closing

post{

 success{
 emailext to: 'devopstrainingblr@gmail.com,mithuntechnologies@yahoo.com',
          subject: "Pipeline Build is over .. Build # is ..${env.BUILD_NUMBER} and Build status is.. ${currentBuild.result}.",
          body: "Pipeline Build is over .. Build # is ..${env.BUILD_NUMBER} and Build status is.. ${currentBuild.result}.",
          replyTo: 'devopstrainingblr@gmail.com'
 }
 
 failure{
 emailext to: 'devopstrainingblr@gmail.com,mithuntechnologies@yahoo.com',
          subject: "Pipeline Build is over .. Build # is ..${env.BUILD_NUMBER} and Build status is.. ${currentBuild.result}.",
          body: "Pipeline Build is over .. Build # is ..${env.BUILD_NUMBER} and Build status is.. ${currentBuild.result}.",
          replyTo: 'devopstrainingblr@gmail.com'
 }
 
}


}
*/



node(){
    
    def mavenHome=tool name: "maven3.9.11"
    
    properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '5', numToKeepStr: '3')), pipelineTriggers([githubPush()])])
    
    stage('checkout'){
        git credentialsId: '81121b4f-64b3-46db-8908-dc33cb0edb99', url: 'https://github.com/akhilmatta89/maven-web-application.git'
    }
    
    stage('CreatePackage'){
        sh "${mavenHome}/bin/mvn clean package"
    }
        
    stage('sonar'){
        sh "${mavenHome}/bin/mvn sonar:sonar"
    }
            
    stage('UploadArtifactsToNexus'){
        sh "${mavenHome}/bin/mvn deploy"
    }
    
    stage('copyToTomcat'){
      sshagent(['6b398c92-3ef8-4e48-868c-d1626f8cac61']) {
        sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@13.61.186.132:/opt/apache-tomcat-9.0.109/webapps/"
        }  
    }
    
stage("SendMail") {
    emailext(
        subject: "Build #${BUILD_NUMBER} - ${JOB_NAME} - ${currentBuild.currentResult}",
        body: """
        <html>
        <body>
            <h2>Jenkins Build Notification</h2>
            <p><b>Job:</b> ${JOB_NAME}</p>
            <p><b>Build Number:</b> ${BUILD_NUMBER}</p>
            <p><b>Status:</b> ${currentBuild.currentResult}</p>
            <p><b>Build URL:</b> <a href="${BUILD_URL}">${BUILD_URL}</a></p>
            <br/>
            <p>Regards,<br/>Jenkins CI/CD</p>
        </body>
        </html>
        """,
        to: 'akhilreddy2672@gmail.com',
        mimeType: 'text/html'
    )
}


}
