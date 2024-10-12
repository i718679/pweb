pipeline{

    parameters {
    choice choices: ['main', 'cloud_unit', 'cloud_intg', 'cloud_accp', 'cloud_prod'], description: 'select the Branch Name', name: 'BranchName'
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
   git branch: 'main', credentialsId: 'c1284e84-c8a1-4766-a110-fef588969026', url: 'https://github.com/i718679/pweb.git'
   }
   }
    stage('Build Package'){
	steps{
	sh "mvn clean package"
	}
	}
	stage('Build Docker Image'){
	steps{
    sh "docker build -t dockervirtual/mavenwebapp ."
    }
    }
	stage('DockerImage Push'){
	steps{
     withCredentials([string(credentialsId: 'Docker hub credentials', variable: 'DOCKER_HUB_CREDENTIALS1')]) {
     sh "docker login -u dockervirtual -p ${DOCKER_HUB_CREDENTIALS1}"
     }
    sh "docker push dockervirtual/mavenwebapp"
    }
    }
	stage('Deploy to Kubernetes Cluster'){
	steps{
    sh "kubectl apply -f mavenwebappdeployment.yaml"
    }
	}
}
}
