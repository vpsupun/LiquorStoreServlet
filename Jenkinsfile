#!goorvy

pipeline {
  agent any
  environment {
    MVN = "/usr/local/apache-maven-3.6.0/bin/mvn"
  }
  stages {
    stage("preflight") {
      steps {
        echo "Preflight"
        sh "ls -al"
      }
    } stage("Build") {
      steps {
        echo "Build"
        sh "${MVN} clean build"
      }
    }
  }
}
