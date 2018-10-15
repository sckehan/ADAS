node {
   def mvnHome
   stage('Preparation') { // for display purposes
      // Get some code from a GitHub repository
      git 'https://github.com/sckehan/cloud-service-validator.git'
      // Get the Maven tool.
      // ** NOTE: This 'M3' Maven tool must be configured
      // **       in the global configuration.           
      //mvnHome = tool 'M3'
   }
   stage('Build') {
      // Run the maven build
 container('build') {
      if (isUnix()) {
          sh './gradlew clean'
      } else {
           bat 'gradlew.bat clean'
      }
   }
   }
}
