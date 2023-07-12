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
		wget https://github.com/mikefarah/yq/releases/download/v4.9.6/yq_linux_amd64.tar.gz
                tar xvf yq_linux_amd64.tar.gz
                mv yq_linux_amd64 /usr/bin/yq
                rm yq_linux_amd64.tar.gz
                git checkout master
                yq eval '.spec.template.spec.containers[0].image = "test:1234567"' -i base/deployment.yaml
                cat overlays/dev/deployment.yaml
                git add overlays/dev/deployment.yaml
                git commit -m 'Change image tag to ${BUILD_NUMBER}'
	    '''

  }

}
}
}
}
