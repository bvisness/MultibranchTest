void setBuildStatus(String message, String state) {
  step([
      $class: "GitHubCommitStatusSetter",
      reposSource: [$class: "ManuallyEnteredRepositorySource", url: "https://github.com/bvisness/MultibranchTest"],
      errorHandlers: [[$class: "ChangingBuildStatusErrorHandler", result: "UNSTABLE"]],
      statusResultSource: [ $class: "ConditionalStatusResultSource", results: [[$class: "AnyBuildResult", message: message, state: state]] ]
  ]);
}

node {
  env.PATH = "${tool 'ant'}\\bin;${env.PATH}"
  withEnv(['JAVA_HOME=C:\\Program Files\\Java\\jdk1.8.0_111']) {
    setBuildStatus("Build #${env.BUILD_NUMBER} in progress", 'PENDING')

    stage ('Checkout') {
      checkout scm
    }
    stage ('Build and Test') {
      try {
        bat 'ant clean-jar'
      } catch (Exception e) {} 
      step([$class: 'JUnitResultArchiver', testResults: 'buildtest/results/*.xml'])
    }
    stage ('Deploy') {
      try {
        bat 'ant deploy'
      } catch (Exception e) {}
    }
    
    echo currentBuild.rawBuild.getResult()
    if (currentBuild.result == 'SUCCESS') {
      setBuildStatus("Build #${env.BUILD_NUMBER} succeeded", "SUCCESS")
    } else {
      setBuildStatus("Build #${env.BUILD_NUMBER} failed", "FAILURE")
    }
  }
}
