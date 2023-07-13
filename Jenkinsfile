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
            NUMBER = "hybrid2k3/petclinic:${BUILD_NUMBER}"
      }
	  steps {
		withCredentials([gitUsernamePassword(credentialsId: 'github-secret', gitToolName: 'git-tool')]) {
		sh '''
	        git clone https://github.com/senatorovv/react-app-deployment.git
  		ls -la
  		cd react-app-deployment
                ls -la
		echo $NUMBER
		git checkout main
		yq eval '.spec.template.spec.containers[0].image = env(NUMBER)' -i overlays/dev/deployment.yml
  		git remote -v
    		git config --global user.email "vsenator@redhat.com"
  		git config --global user.name "Viktor"
    		git branch -M main
  		git add overlays/dev/deployment.yml
   		git commit -m 'Change image tag'
                git push --set-upstream origin main
		cd .. && rm -rf react-app-deployment
		'''
}
  }

}
}
}
