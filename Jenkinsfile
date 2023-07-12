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
	  steps {
	    sh '''
		git clone https://github.com/senatorovv/react-app-deployment.git
  
  		wget https://github.com/mikefarah/yq/releases/download/v4.9.6/yq_linux_amd64.tar.gz
                rm -rf react-app-deployment
	    '''

  }

}
}
}
