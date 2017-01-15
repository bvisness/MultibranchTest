node {
  stage ('Build and Test') {
    env.PATH = "${tool 'ant'}\\bin;${env.PATH}"
    echo env.PATH
    checkout scm
    withEnv(['JAVA_HOME=C:\\Program Files\\Java\\jdk1.8.0_111']) {
      try {
        bat 'ant clean-jar'
      } catch (Exception e) {}
      step([$class: 'JUnitResultArchiver', testResults: 'buildtest/results/*.xml'])
    }
  }
}
