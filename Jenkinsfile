pipeline {
  environment{
  rigistry= "naveen047/maven-app"
  registryCredential= "Dockerhub-Credential"
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
             docker.build rigistry + ":$BUILD_NUMBER"
           }
       }
    }

     stage('Pushing Image'){
       steps{
          script{
            docker.withRegistry ('', Dockerhub-Credential ){
              dockerImage.push()
            }
          }
       }
     }

    stage('Deploying Spring container to Kubernetes') {
      steps {
        script {
          kubernetesDeploy(configs: "deployment.yaml", "service.yaml")
        }
      }
    }
  }
}
