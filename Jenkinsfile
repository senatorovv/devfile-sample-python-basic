pipeline {

  environment {
    dockerimagename = "hybrid2k3/petclinic"
    dockerImage = ""
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
	environment {
        GIT_CREDS = credentials('github-secret')
	  steps {
	    sh '''
		git clone https://github.com/senatorovv/react-app-deployment.git
	    '''

  }

}
}
}
}
