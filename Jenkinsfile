#!goorvy
@Library('jenkins-shared-libraries') _

Map teraMap = [branch:"baseg", credentialsId:"afe23124-6763-4fec-9810-95ca66beea10", url:"https://github.com/vpsupun/shared-terraform.git" ]
Map ansiMap = [branch:"baseg", credentialsId:"afe23124-6763-4fec-9810-95ca66beea10", url:"https://github.com/vpsupun/shared-ansible.git", ansibleKey_credentialsId:"ansibleKey", ansibleUser:"centos" ]


pipeline {
  agent any
  environment {
	  MVN = "/usr/local/apache-maven-3.6.0/bin/mvn"   
	  AWS_DEFAULT_REGION    = "ap-southeast-1"     
	  TF_VAR_count          = 1
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
    stage("Deploy Test") {
      steps {
        echo "Deploy Test"
        withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'sia_creds']]) {
	echo "Terraform provisioning"
        terraformRun(teraMap, "deploy")
	echo "Ansible deployment"
	ansibleRun(ansiMap, "deploy.yml", "./target/SampleServlet.war")
        }
      }
    }
    stage("Functional Tests") {
      steps {
        echo "Functional Tests"
        echo "Placeholder for Functional Tests"
	sh "sleep 60"
      }
    }
    stage("Tear Down") {
      steps {
        echo "Tear Down"
	withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'sia_creds']]) {
	echo "Going in"
        terraformRun(teraMap, "destroy")
        }
      }
    }
    stage('Publishing') {
      steps {
        nexusPublisher nexusInstanceId: 'localNexus', nexusRepositoryId: 'sia', packages: [[$class: 'MavenPackage', mavenAssetList: [[classifier: '', extension: '', filePath: './target/SampleServlet.war']], mavenCoordinate: [artifactId: 'SampleServlet', groupId: 'com.sia', packaging: 'war', version: '1.1']]] 
      }
    }
  }
}
