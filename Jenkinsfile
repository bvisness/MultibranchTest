node {
  stage ('Build and Test') {
    env.PATH = "${tool 'ant'}\bin;${env.PATH}"
    echo env.PATH
    checkout scm
    bat 'ant clean-jar'
  }
}
