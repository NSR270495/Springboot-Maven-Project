pipeline {
  environment{
  dockerimagename = "naveen047/maven-app"
  dockerImage = ""
  }
  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git url:'https://github.com/NSR270495/Springboot-Maven-Project.git', branch:'main'
      }
    }

    stage('Build image'){
       steps{
           script{
             dockerImage=docker.build dockerimagename + ":$BUILD_NUMBER"
           }
       }
    }

     stage('Pushing Image'){
       environment{
         rigistryCredential = "Dockerhub-Credential"
       }
       steps{
          script{
            docker.withRegistry ('https://registry.hub.docker.com', rigistryCredential ){
              dockerImage.push("latest")
            }
          }
       }
     }

    stage('Deploying Spring container to Kubernetes') {
      steps {
        withKubeConfig([credentialsId: 'kubeconfig']) {
          sh 'cat Deployment.yaml | sed "s/{{BUILD_NUMBER}}/$BUILD_NUMBER/g" | kubectl apply -f -'
          sh 'kubectl apply -f Service.yaml'
        }
      }
    }
  }
}
