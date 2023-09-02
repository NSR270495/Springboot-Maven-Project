pipeline {

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git url:'https://github.com/NSR270495/Springboot-Maven-Project.git', branch:'main'
      }
    }

    stage('Build image'){
       steps{
           withCredentials([usernamePassword(credentialsId: 'Dockerhub-Credential', passwordVariable: 'Password', usernameVariable: 'Username')]) {
           sh "docker build -t maven-app:$BUILD_NUMBER ."
           sh "docker tag maven-app:$BUILD_NUMBER naveen047/maven-app:$BUILD_NUMBER"
           }
       }
    }

     stage('Pushing Image'){
       steps{
          withCredentials([usernamePassword(credentialsId: 'Dockerhub-Credential', passwordVariable: 'Password', usernameVariable: 'Username')]) {
             sh "docker push naveen047/maven-app:$BUILD_NUMBER"
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

  }

}
