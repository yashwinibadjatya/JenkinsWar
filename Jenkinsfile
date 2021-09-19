node{

   def tomcatWeb = 'C:\\Program Files\\Apache Software Foundation\\Tomcat 9.0\\webapps'
   def tomcatBin = 'C:\\Program Files\\Apache Software Foundation\\Tomcat 9.0\\bin'
   def tomcatStatus = ''
   def EMAIL_TO = 'saanvijain1718@gmail.com'
   stage('SCM Checkout'){
     git 'https://github.com/yashwinibadjatya/JenkinsWar.git'
   }
   stage('Compile-Package-create-war-file'){
      // Get maven home path
      bat(/"D:\Software\apache-maven-3.8.2-bin\apache-maven-3.8.2\bin\mvn" package/)
      }
    stage('SonarQube Analysis'){
         def scannerHome = tool 'sonarqube';
         withSonarQubeEnv('sonarqube'){
             bat "${scannerHome}/bin/sonar-scanner \
             -D sonar.login=admin \
             -D sonar.password=root \
             -D sonar.projectKey=com.jenkins.demo:JenkinsWar1 \
             -D sonar.exclusions=vendor/**,resources/**,**/*.java \
             -D sonar.host.url=http://localhost:9000"
         }
     } 
      stage("Quality Gate"){
          timeout(time: 1, unit: 'HOURS') {
              def qg = waitForQualityGate()
              if (qg.status != 'OK') {
                   mail bcc: '', body: '''Hello,
                  Thanks''', cc: '', from: '', replyTo: '', subject: "${BUILD_URL} ${BUILD_NUMBER}", to: 'saanvijain1718@gmail.com'
                  error "Pipeline aborted due to quality gate failure: ${qg.status}"
              }
          }
      } 
    //   stage('Email Notification'){
    //       mail bcc: '', body: '''Hello,
    //       Thanks''', cc: '', from: '', replyTo: '', subject: 'Jenkins Email Test', to: 'saanvijain1718@gmail.com'
    //   }
      stage('Email Notification') {
          echo "hiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiii- ${currentBuild.currentResult}"
          if (currentBuild.currentResult == 'FAILURE') {
              emailext body: 'Check console output at $BUILD_URL to view the results. \n\n ${CHANGES} \n\n -------------------------------------------------- \n${BUILD_LOG, maxLines=100, escapeHtml=false}', 
                    to: "${EMAIL_TO}", 
                    subject: 'Build failed in Jenkins: $PROJECT_NAME - #$BUILD_NUMBER'
          } else if (currentBuild.currentResult == 'UNSTABLE') {
              emailext body: 'Check console output at $BUILD_URL to view the results. \n\n ${CHANGES} \n\n -------------------------------------------------- \n${BUILD_LOG, maxLines=100, escapeHtml=false}', 
                    to: "${EMAIL_TO}", 
                    subject: 'Unstable build in Jenkins: $PROJECT_NAME - #$BUILD_NUMBER'
               
          }else if (currentBuild.currentResult == 'CHANGED') {
              emailext body: 'Check console output at $BUILD_URL to view the results.', 
                    to: "${EMAIL_TO}", 
                    subject: 'Jenkins build is back to normal: $PROJECT_NAME - #$BUILD_NUMBER'
               
          } else if (currentBuild.currentResult == 'SUCCESS') {
              mail bcc: '', body: '''Hello,
             Thanks''', cc: '', from: '', replyTo: '', subject: "${BUILD_URL} ${BUILD_NUMBER}", to: 'saanvijain1718@gmail.com'
          }
      }


   stage('Deploy to Tomcat'){
     bat "copy target\\JenkinsWar.war \"${tomcatWeb}\\JenkinsWar.war\""
   }
      
}
