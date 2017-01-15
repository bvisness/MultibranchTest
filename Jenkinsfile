void setBuildStatus(String message, String state) {
  step([
      $class: "GitHubCommitStatusSetter",
      // reposSource: [$class: "ManuallyEnteredRepositorySource", url: "https://github.com/my-org/my-repo"],
      // contextSource: [$class: "ManuallyEnteredCommitContextSource", context: "ci/jenkins/build-status"],
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
    withEnv(['JAVA_HOME=C:\\Program Files\\Java\\jdk1.8.0_111']) {
      try {
        bat 'ant clean-jar'
        setBuildStatus("Tests passed", "SUCCESS")
      } catch (Exception e) {
        setBuildStatus("Tests failed", "FAILURE")
      } 
      step([$class: 'JUnitResultArchiver', testResults: 'buildtest/results/*.xml'])
    }
  }
}
