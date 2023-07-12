pipeline {

  environment {
    dockerimagename = "hybrid2k3/petclinic"
    dockerImage = ""
    BUILD_NUMBER = "${env.BUILD_NUMBER}"
    myvar = "hybrid2k3/petclinic:$BUILD_NUMER"
  }

  agent any

  stages {

       stage('Build image') {
      steps{
        script {
          dockerImage = docker.build dockerimagename
        }
      }
    }

    stage('Pushing Image') {
      environment {
               registryCredential = 'dockerHub'
           }
      steps{
        script {
          docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) {
            dockerImage.push("${BUILD_NUMBER}")
          }
        }
      }
    }
	
	
	stage('Change the deployment') {
	  steps {
    		withEnv(['MY_NAME_IS=$BUILD_NUMBER']) {
		sh "yq eval '.spec.template.spec.containers[0].image = "hybrid2k3/petclinic:$MY_NAME_IS"' -i base/deployment.yml"
  }
	}
  }

}
}
