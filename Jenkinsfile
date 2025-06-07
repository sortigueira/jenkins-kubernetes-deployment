pipeline {

  environment {
    dockerimagename = "sortigueira/react-app"
    dockerImage = ""
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
          dockerImage = docker.build dockerimagename
        }
      }
    }

    stage('Pushing Image') {
      environment {
               registryCredential = 'dockerhub-credentials'
           }
      steps{
        script {
          docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) {
            dockerImage.push("latest")
          }
        }
      }
    }

    stage('Deploying React.js container to Kubernetes') {
      steps {
        script {
          kubernetesDeploy(configs: "deployment.yaml", "service.yaml")
        }
      }
    }

    stage('Deploying React.js container to Kubernetes') {
      steps {
        script {
          // Si kubectl no está en el mismo contenedor que docker, podrías necesitar otro `agent` o `container`
          // dependiendo de cómo estructures tus Pod Templates.
          // Por ejemplo, si tu Pod Template 'docker-builder' también incluye kubectl:
          kubernetesDeploy(configs: "deployment.yaml", "service.yaml")
        }
      }
    }

  }



}
