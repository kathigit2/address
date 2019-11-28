pipeline {
  agent any
  tools { 
        jdk 'JAVA_HOME'
        maven 'Maven'
  }
  stages {
    stage('Clone repository') {
        /* Let's make sure we have the repository cloned to our workspace... */
      steps {
        checkout scm
      }
    }
    stage('Build') {
      steps {
        sh 'mvn -B -DskipTests clean package'
        sh 'echo $USER'
        sh 'echo whoami'
      }
      stage('Docker Build') {
      steps {
        sh '/usr/bin/docker build -t address-service .'
      }
    }
   
    stage('push image to ECR'){
      steps {
       withDockerRegistry(credentialsId: 'ecr:ap-south-1:aws-credentials', url: 'http://887625267599.dkr.ecr.ap-south-1.amazonaws.com/address-service') {
          sh 'docker tag address-service:latest 887625267599.dkr.ecr.ap-south-1.amazonaws.com/address-service:latest'
          sh 'docker push 887625267599.dkr.ecr.ap-south-1.amazonaws.com/address-service:latest'
       }
      }
      
