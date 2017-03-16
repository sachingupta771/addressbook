node {
   def mvnHome
   def version 
   stage('Preparation') {
      git 'https://github.com/sachingupta771/addressbook.git'
      mvnHome = tool 'LOCAL_MAVEN'
	  version = '3.3.9' 
   }
   stage('Perform-UnitTest') {
        withMaven(
        maven: 'LOCAL_MAVEN', // Maven installation declared in the Jenkins "Global Tool Configuration"
        mavenSettingsConfig: 'settings.xml', // Maven settings.xml file defined with the Jenkins Config File Provider Plugin
        mavenLocalRepo: '/opt/maven') {

      if (isUnix()) {
         sh "'${mvnHome}/bin/mvn' -Dmaven.test.failure.ignore clean test"
      } else {
         bat(/"${mvnHome}\bin\mvn" -Dmaven.test.failure.ignore clean test/)
      }
    } // withMaven will discover the generated Maven artifacts, JUnit reports and FindBugs reports
    

   }
   stage('PublishResults') {
      junit '**/target/surefire-reports/TEST-*.xml'
      archive 'target/*.jar'
   }

   stage('notify') { 
   } 
} 
