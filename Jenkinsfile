node {

def mvnHome = tool 'Maven3'
stage ('Checkout') {

checkout scm
}
stage ('Build') {
sh "${mvnHome}/bin/mvn clean install -f MyAwesomeApp/pom.xml"
}
stage ('Code Quality Scan') {
withSonarQubeEnv('SonarQube') {
sh "${mvnHome}/bin/mvn sonar:sonar -f MyAwesomeApp/pom.xml"
   }
}

    }

   stage ('DEV Deploy')  {
      echo "deploying to DEV Env "
      deploy adapters: [tomcat9(credentialsId: '7b527f77-3ef1-4d11-b552-748de9b7ed7b', path: '', url: 'http://34.205.24.116:8080')], contextPath: null, war: '**/*.war'
    }
stage ('DEV Approve') {
echo "Taking approval from DEV"
timeout(time: 7, unit: 'DAYS') {
input message: 'Do you want to deploy?', submitter: 'admin'
     }
 }


stage ('Slack Notification') {


    slackSend(channel:'#myawesomeapp', message: "Job is successful, here is the info -  Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")


  }
