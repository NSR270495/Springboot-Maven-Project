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
        sshagent(['k8s-ssh-key']) {
           sh 'ssh -o StrictHostKeyChecking=no ec2-user@13.233.104.249 cd /home/ec2-user'
           sh 'scp -O StrictHostKeyChecking=no Deployment.yaml ec2-user@13.233.104.249 .
        }
      }
    }
  }
}
