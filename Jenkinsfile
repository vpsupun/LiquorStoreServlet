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
        sh "${MVN} clean install -DskipTests"
      }
    }
    stage("Unit Tests") {
      steps {
        echo "Unit Tests"
        sh "${MVN} install"
      }
    }
    stage("Static Analysis") {
      steps {
        parallel(
          SonarScan: {
            echo "Placeholder for Sonar Scan"
          },
          FortifyScan: {
            echo "Placeholder for Fortify Security Scan"
          }
        )
      }
    }
    stage("Nexus IQ Scan") {
      steps {
        echo "Nexus IQ Scan"
        echo "Placeholder for Nexus IQ Scan"
      }
    }
    stage('Publishing') {
      steps {
        nexusPublisher nexusInstanceId: 'localNexus', nexusRepositoryId: 'sia', packages: [[$class: 'MavenPackage', mavenAssetList: [[classifier: '', extension: '', filePath: './target/SampleServlet.war']], mavenCoordinate: [artifactId: 'SampleServlet', groupId: 'com.sia', packaging: 'war', version: '1.1']]] 
      }
    }
  }
}
