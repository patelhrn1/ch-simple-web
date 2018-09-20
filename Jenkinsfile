node {
   def mvnHome
   stage('Preparation') { // for display purposes
      // Get some code from a GitHub repository
      git 'https://github.com/jglick/simple-maven-project-with-tests.git'
      // Get the Maven tool.
      // ** NOTE: This 'M3' Maven tool must be configured
      // **       in the global configuration.           
      mvnHome = tool 'Maven'
   }
   stage('Build') {
      // Run the maven build
      if (isUnix()) {
         sh "'${mvnHome}/bin/mvn' -Dmaven.test.failure.ignore clean package"
      } else {
         bat(/"${mvnHome}\bin\mvn" -Dmaven.test.failure.ignore clean package/)
      }
   }
   stage('CodeAnalysis'){
   bat 'mvn sonar:sonar'
   }
   stage('Input'){
   timeout(time: 60, unit: 'SECONDS') {
      input 'Should we proceed?'
      }
   }
   stage('Results') {
      junit '**/target/surefire-reports/TEST-*.xml'
      archive 'target/*.jar'
   }
  stage('Deploy') {
         when {
       expression {
     currentBuild.result == null || currentBuild.result == 'SUCCESS' ①
       }
  }
  steps {
      echo "Deploying..."
  }
  }
}
