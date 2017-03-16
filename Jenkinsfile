node {
   def mvnHome

   def version 
   stage('Preparation') {
      git 'https://github.com/SeshagiriSriram/addressbook.git'
      mvnHome = tool 'LOCAL_MAVEN'
	  version = '2.3.5' 
   }
   stage('PerformUnitTest') {
        withMaven(
        maven: 'LOCAL_MAVEN', // Maven installation declared in the Jenkins "Global Tool Configuration"
        mavenSettingsConfig: 'settings.xml', // Maven settings.xml file defined with the Jenkins Config File Provider Plugin
        mavenLocalRepo: 'd:/repos') {

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

   stage('NotifySachinandCo') { 
   } 
} 

   stage('Preparation') { // for display purposes
      // Get some code from a GitHub repository
      git 'https://github.com/beingpratik/addressbook.git'
      // Get the Maven tool.
      // ** NOTE: This 'M3' Maven tool must be configured
      // **       in the global configuration.           
      mvnHome = tool 'LOCAL_MAVEN'
      
    
      version = '2.3.5'
   }
   stage('Build') {
      // Run the maven build
      if (isUnix()) {
         sh "'${mvnHome}/bin/mvn' -Dmaven.test.failure.ignore clean package"
      } else {
         bat(/"${mvnHome}\bin\mvn" -Dmaven.test.failure.ignore clean package/)
      }
   }
   stage('Results') {
      junit '**/target/surefire-reports/TEST-*.xml'
      //archive 'target/*.jar'
   }
   stage('Publish') {
     nexusPublisher nexusInstanceId: 'NEXUS', nexusRepositoryId: 'release_to_qa', packages: [[$class: 'MavenPackage', mavenAssetList: [[classifier: '', extension: '', filePath: 'addressbook_main/target/addressbook.war']], mavenCoordinate: [artifactId: 'addressbook_main', groupId: 'com.edurekademo.tutorial', packaging: 'war', version: '2.3.0']]]
   }
}
