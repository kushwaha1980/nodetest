pipeline {
  environment{
    dockerimagename = "kumard31/nodeapp"
    dockerimage = ""

  }

  agent any

  stages{
    
    stage('Checkout source-code') {
      
      steps{
        git branch: 'main', url: 'https://github.com/kushwaha1980/nodedocker_app.git'

      }
    }
    stage('Build-image') {
      
      steps{
        script{
          dockerimage = docker.build (dockerimagename)
        }
        
      }

    }

    stage('pushing-image') {
      environment{
        registryCredential = 'dockerhublogin'
      }
      steps{
        script{
          
          docker.withRegistry('https://registry.hub.docker.com', registryCredential){
            dockerimage.push('latest')
          }
        }
      }
    }
    stage("deploying image to k8s") {

      steps{
        script{
          kubernetesDeploy(configs: "deploymentservice.yml", kubeconfigId: "kubernetes")
        }
      }

    }

  }


}
