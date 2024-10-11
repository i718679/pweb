pipeline{

parameters {
  choice choices: ['cloud_unit', 'cloud_intg', 'cloud_accp', 'cloud_prod'], description: 'select the Branch Name', name: 'BranchName'
  string defaultValue: 'Harikrishna Annam', name: 'PersonName'
}

agent any

tools{
maven 'Maven'
}

options {
  buildDiscarder logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '', numToKeepStr: '50')
  timestamps()
}

stages{
   stage('CheckoutCode'){
   steps{
   git branch: "${params.BranchName}", credentialsId: '960a1bf2-9705-4205-8804-a856f4ad3acd', url: 'https://github.com/voyafinance/wis-apache-external.git'
   }
   }
    stage('Build'){
	steps{
	sh "mvn clean package"
	}
	}
	stage('SonarQubeReport'){
	steps{
	sh "mvn clean sonar:sonar"
	}
	}
	stage('UploadArtifacts'){
	steps{
	sh "mvn clean deploy"
	}
	}
	stage('Deploy Application'){
	steps{
	sshagent(['3e7930c7-63f6-4994-a84a-aae18745aecf']) {
    sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@172.31.0.183:/opt/apache-tomcat-9.0.95/webapps/"
	}
	}
	}
	
}//stages closing
}//pipeline closing
