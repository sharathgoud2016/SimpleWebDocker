pipeline{
  agent { node { label 'RedHatSlave01'} }
  tools {
      git 'git'
      maven 'maven'
  }

  environment {
    IMAGE_TAG = "${env.BUILD_NUMBER}"
  }
stages{
   stage('SCM Checkout'){
     steps{
       git 'https://github.com/sharathgoud2016/SimpleWebDocker.git'
     }
   }
   stage('WAR Build'){
     steps{
           sh label: '', script: 'mvn clean package'
        }
    }

   stage('Approval to Deploy') {
     steps {
        input('Do you want to proceed to dev?')
     }
    }
   stage('Docker Build') {
     steps{
        withCredentials([string(credentialsId: '7cd68205-b062-4b47-8751-d7b48a165b89', variable: 'DOCKER_PWD')]) {
          sh '''
		  sudo docker login -u sharathgoud2016 -p ${DOCKER_PWD} docker.io
		  sudo docker build -t docker.io/sharathgoud2016/simplewebapp01:${IMAGE_TAG} .
		  sudo docker push docker.io/sharathgoud2016/simplewebapp01:${IMAGE_TAG}
		  sudo docker images
		  sudo docker run -d -p 8080:8080 --name simplewebapp01 sharathgoud2016/simplewebapp01:${IMAGE_TAG}
		  #sudo docker rmi -f docker.io/sharathgoud2016/simplewebapp01:${IMAGE_TAG}
		  sudo docker logout docker.io
		  '''
        }
     }
  }
 
 }
}