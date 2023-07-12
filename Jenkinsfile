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
  		cd react-app-deployment
		git checkout main
		yq eval '.spec.template.spec.containers[0].image = "test:1234567"' -i base/deployment.yaml
  		git add base/deployment.yaml
   		git commit -m 'Change image tag'
     		git push
                rm -rf react-app-deployment
	    '''

  }

}
}
}
