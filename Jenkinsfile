pipeline {
  environment{
    registry = "vinay4790/testrepo"
    registryCredential = 'dockerhub'
    dockerImage = ''
    containerId = sh(script: 'docker ps -aqf "name=node-app"', returnStdout:true)	
	 
   
  }
  agent any
  
    
  stages {
    stage('Cloning Git') {
      steps {
        git 'https://github.com/vinaybp/docker-hello-world-spring-boot'
      }
    }
        
    stage('Build') {
      steps {
        sh "'${mvnHome}/bin/mvn' -Dmaven.test.failure.ignore clean package"
      }
    }
     
   /* stage('Test') {
      steps {
         sh 'npm test'
      }
    }*/
    stage('Building Image'){
      steps{
        script{
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }
    
    stage('Push Image'){
      steps{
        script{
          docker.withRegistry('',registryCredential) {
            dockerImage.push()
          }
        }
      }
    }
    
    stage('Cleanup'){
      when{
        not {environment ignoreCase:true, name:'containerId', value:''}
      }
      steps {
        sh 'docker stop ${containerId}'
        sh 'docker rm ${containerId}'
      }
    }
    stage('Run Container'){
      steps{
        sh 'docker run --name=node-app -d -p 3000:3000 $registry:$BUILD_NUMBER &'
      }
    }
    
   
    
  }
}
