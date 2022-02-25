pipeline {

  environment {
    dockerimagename = "srijaldocker/nodechat"
    dockerImage = ""
  }

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git 'https://github.com/SrijalKarmacharya/node-chat.git'
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
               registryCredential = 'dockerhublogin'
           }
      steps{
        script {
          docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) {
            dockerImage.push("latest")
          }
        }
      }
    }

    stage('Deploying App to Kubernetes') {
      steps {
//        script {
//          kubernetesDeploy(configs: "deploymentservice.yml", kubeconfigId: "kubernetes")
//        }
        withCredentials([
        file(credentialsId: "kubeconfig", variable: 'KUBECRED')
        
    ]) {
        sh """
       export KUBECONFIG=$KUBECRED
       kubectl apply -f deploymentservice.yml
        """
    
        }
      }
    }

  }

}
