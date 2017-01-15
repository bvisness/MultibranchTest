void setBuildStatus(String message, String state) {
  step([
      $class: "GitHubCommitStatusSetter",
      errorHandlers: [[$class: "ChangingBuildStatusErrorHandler", result: "UNSTABLE"]],
      statusResultSource: [ $class: "ConditionalStatusResultSource", results: [[$class: "AnyBuildResult", message: message, state: state]] ]
  ]);
}

node {
  env.PATH = "${tool 'ant'}\\bin;${env.PATH}"
  
  stage ('Checkout') {
    checkout scm
  }
  stage ('Build and Test') {
    setBuildStatus("Build #${env.BUILD_NUMBER} in progress", "PENDING")
    withEnv(['JAVA_HOME=C:\\Program Files\\Java\\jdk1.8.0_111']) {
      try {
        bat 'ant clean-jar'
        setBuildStatus("Build #${env.BUILD_NUMBER} succeeded", "SUCCESS")
      } catch (Exception e) {
        setBuildStatus("Build #${env.BUILD_NUMBER} failed", "FAILURE")
      } 
      step([$class: 'JUnitResultArchiver', testResults: 'buildtest/results/*.xml'])
    }
  }
}
