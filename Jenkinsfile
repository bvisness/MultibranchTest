node {
  stage ('Build and Test') {
    env.PATH = "${tool 'ant'}\\bin;${env.PATH}"
    echo env.PATH
    checkout scm
    withEnv(['JAVA_HOME=C:\\Program Files\\Java\\jdk1.8.0_111']) {
      bat 'ant clean-jar'
      step([$class: 'JUnitResultArchiver', testResults: 'buildtest/results/*.xml'])
    }
  }
}
