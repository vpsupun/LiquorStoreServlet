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
    }
    stage("Build") {
      steps {
        echo "Build"
        sh "${MVN} clean install"
      }
    }
    stage('Publishing') {
      steps {
        nexusPublisher nexusInstanceId: 'localNexus', nexusRepositoryId: 'sia', packages: [[$class: 'MavenPackage', mavenAssetList: [[classifier: '', extension: '', filePath: './target/SampleServlet.war']], mavenCoordinate: [artifactId: 'SampleServlet', groupId: 'com.sia', packaging: 'war', version: '1.1']]] 
      }
    }
  }
}
