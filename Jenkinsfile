pipeline {

  environment {
    dockerimagename = "sortigueira/react-app"
    BRANCH_NAME = "main"
  }

  agent {
      label 'docker-builder' 
  }
  
  stages {

    stage('Checkout Source') {
      steps {
        git 'https://github.com/sortigueira/jenkins-kubernetes-deployment.git'
      }
    }

    stage('Build image') {
      steps{
        script {
          container('docker-cli') {
            sh "docker build -t ${dockerimagename}:latest ."
          }
        }
      }
    }

    stage('Pushing Image') {
      environment {
            registryCredential = 'dockerhub-credentials'
          }
      steps{
        script {
          container('docker-cli') { 
            withCredentials([usernamePassword(credentialsId: registryCredential, passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
              sh "docker login -u \"$DOCKER_USERNAME\" -p \"$DOCKER_PASSWORD\""
              sh "docker push ${dockerimagename}:latest"
            }
          } 
        }
      }
    }

    stage('Deploying React.js container to Kubernetes') {
      steps {
        script {
          container('kubectl-cli') {
            sh "kubectl apply -f deployment.yaml"
            sh "kubectl apply -f service.yaml"
          }
          
        }
      }
    }

  }



}
