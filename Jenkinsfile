#!goorvy
@Library('jenkins-shared-libraries') _

pipeline {
  agent any
  environment {
    MVN = "/usr/local/apache-maven-3.6.0/bin/mvn"
	
	  AWS_ACCESS_KEY_ID     = credentials("aws_access_key")     
	  AWS_SECRET_ACCESS_KEY = credentials("aws_secret_key")     
	  AWS_DEFAULT_REGION    = "us-west-2"     
	  TF_VAR_count          = "${params.distributed_nodes}"
  }
  stages {
    stage("preflight") {
      steps {
        echo "Preflight"
        sh "ls -al"
	terraform(name:"Test Name")
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
	stage("Deploy Test") {
      steps {
        echo "Deploy Test"
 
      }
    }
	stage("Functional Tests") {
      steps {
        echo "Functional Tests"
        echo "Placeholder for Functional Tests"
      }
    }
	stage("Tear Down") {
      steps {
        echo "Tear Down"

      }
    }
    stage('Publishing') {
      steps {
        nexusPublisher nexusInstanceId: 'localNexus', nexusRepositoryId: 'sia', packages: [[$class: 'MavenPackage', mavenAssetList: [[classifier: '', extension: '', filePath: './target/SampleServlet.war']], mavenCoordinate: [artifactId: 'SampleServlet', groupId: 'com.sia', packaging: 'war', version: '1.1']]] 
      }
    }
  }
}
