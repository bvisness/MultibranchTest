node {
  echo "${tool 'ant'}"
  stage ('Build and Test') {
    env.PATH = "${tool 'ant'}\bin;${env.PATH}"
    checkout scm
    bat 'ant clean-jar'
  }
}
