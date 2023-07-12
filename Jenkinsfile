pipeline {

  environment {
    dockerimagename = "hybrid2k3/petclinic"
    dockerImage = ""
    BUILD_NUMBER = "${env.BUILD_NUMBER}"
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
		withCredentials([gitUsernamePassword(credentialsId: 'github-secret', gitToolName: 'git-tool')]) {
		sh '''
	        git clone https://github.com/senatorovv/react-app-deployment.git
  		ls -la
  		cd react-app-deployment
                ls -la
		echo $BUILD_NUMBER
  		echo ${BUILD_NUMBER}
  		git checkout main
		yq eval '.spec.template.spec.containers[0].image = "hybrid2k3/petclinic:$BUILD_NUMER"' -i base/deployment.yml
  		git remote -v
    		git config --global user.email "vsenator@redhat.com"
  		git config --global user.name "Viktor"
    		git branch -M main
  		git add base/deployment.yml
   		git commit -m 'Change image tag'
                git push --set-upstream origin main
		rm -rf react-app-deployment
		'''
}
  }

}
}
}
