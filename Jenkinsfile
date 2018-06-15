// see https://dzone.com/refcardz/continuous-delivery-with-jenkins-workflow for tutorial
// see https://documentation.cloudbees.com/docs/cookbook/_pipeline_dsl_keywords.html for dsl reference
// This Jenkinsfile should simulate a minimal Jenkins pipeline and can serve as a starting point.
// NOTE: sleep commands are solelely inserted for the purpose of simulating long running tasks when you run the pipeline
node {
   // Mark the code checkout 'stage'....
   stage 'checkout'

   // Get some code from a GitHub repository
   git url: 'https://github.com/walidg2/jenkinsfile'
   bat 'git clean -fdx & timeout 4'

   // Get the maven tool.
   // ** NOTE: This 'mvn' maven tool must be configured
   // **       in the global configuration.
   def mvnHome = tool 'mvn'

   stage 'build'
   // set the version of the build artifact to the Jenkins BUILD_NUMBER so you can
   // map artifacts to Jenkins builds
   bat "${mvnHome}/bin/mvn versions:set -DnewVersion=${env.BUILD_NUMBER}"
   bat "${mvnHome}/bin/mvn package"

   stage 'test'
   parallel 'test': {
     bat "${mvnHome}/bin/mvn test & timeout 2"
   }, 'verify': {
     bat "${mvnHome}/bin/mvn verify & timeout 3"
   }

   stage 'archive'
   archive 'target/*.jar'
}


node {
   stage 'deploy Canary'
   bat 'echo "write your deploy code here" & timeout 5'

   stage 'deploy Production'
   input 'Proceed?'
   bat 'echo "write your deploy code here" & timeout 6'
   archive 'target/*.jar'
}
