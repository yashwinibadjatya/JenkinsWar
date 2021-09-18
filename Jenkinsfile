node{

   def tomcatWeb = 'C:\\Program Files\\Apache Software Foundation\\Tomcat 9.0\\webapps'
   def tomcatBin = 'C:\\Program Files\\Apache Software Foundation\\Tomcat 9.0\\bin'
   def tomcatStatus = ''
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
             -D sonar.projectKey=jenkins-pipeline \
             -D sonar.exclusions=vendor/**,resources/**,**/*.java \
             -D sonar.host.url=http://localhost:9000/"
         }
     } 
   stage('Sonarqube Quality Gate'){
     waitForQualityGate abrotPipeline: true
   }
/*   stage ('Stop Tomcat Server') {
               bat ''' @ECHO OFF
               wmic process list brief | find /i "tomcat" > NUL
               IF ERRORLEVEL 1 (
                    echo  Stopped
               ) ELSE (
               echo running
                  "${tomcatBin}\\shutdown.bat"
                  sleep(time:10,unit:"SECONDS") 
               )
'''
   }*/
   stage('Deploy to Tomcat'){
     bat "copy target\\JenkinsWar.war \"${tomcatWeb}\\JenkinsWar.war\""
   }
      
}
