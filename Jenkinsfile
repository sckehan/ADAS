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
   /*
   stage('Build') {
      // Run the maven build
      if (isUnix()) {
          sh './gradlew clean'
      } else {
           bat 'gradlew.bat clean'
      }
   }
   */
   def server
   def buildInfo
   def rtGradle
   /*
   stage('Artifactory Configuration'){
     //def server = Artifactory.newServer url: "http://192.168.248.170:8083/artifactory", credentialsId: 'jfrog-artifactory'
     //def buildInfo
      server= Artifactory.server "artifactory"
      rtGradle = Artifactory.newGradleBuild()
      rtGradle.deployer repo:'aliyun', server: server
      rtGradle.tool = "gradle"
   }
     */
   stage('Gradle build'){
    server= Artifactory.server "artifactory"
    rtGradle = Artifactory.newGradleBuild()
    rtGradle.resolver server: server, repo: 'libs-release'
    rtGradle.deployer server: server, repo: 'libs-release-local'

    //Use Gradle Wrapper
    rtGradle.useWrapper = true

    //Creates buildinfo
    buildInfo = Artifactory.newBuildInfo()
    buildInfo.env.capture = true
    buildInfo.env.filter.addInclude("*")

    // Run Gradle:
    rtGradle.run rootDir: "./", buildFile: 'build.gradle', tasks: 'clean artifactoryPublish', buildInfo: buildInfo

    // Publish the build-info to Artifactory:
    server.publishBuildInfo buildInfo
      
       //buildInfo = rtGradle.run rootDir: "aliyun/", buildFile: 'build.gradle', tasks: 'clean artifactoryPublish'
   }
   /*
   stage ('Publish build info') {
        server.publishBuildInfo buildInfo
    }
   */
}
