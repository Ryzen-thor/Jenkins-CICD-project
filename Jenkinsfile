pipeline { 
	agent any 
	
	environment  {
		IMAGE_TAG = "${BUILD_NUMBER}"
	}

	stages {

	  stage('Checkout') {
	     steps {
	       git 'https://github.com/Ryzen-thor/Jenkins-CICD-project.git'
	      }

	     }

	  stage('Build Docker'){
		steps{
			script{
				sh '''
				echo 'Build Docker Image'
				docker build -t stormbreaker2023/todo-app:IMAGE_TAG .
				'''
			}

		}
		
	  }

	  stage('Push the artifacts'){
		steps{
			script{
				sh '''
					echo 'push to DockerHub'
					docker push stormbreaker2023/todo-app:IMAGE_TAG
				'''
			}
		}
	  }

	  stage('Update k8s manifest and push to repo'){
		steps{
			script{
				sh '''
				ch deploy
				cat deployment.yaml
				sed -i '' "s/v2/${BUILD_NUMBER}/g" deployment.yaml
				cat deployment.yaml
				git add deployment.yaml
				git commit -m 'Updated deployment.yaml | using Jenkinsfile pipeline'
				git remote -v
				git push 

				'''
			}
		}
	  }
	 }

}	
