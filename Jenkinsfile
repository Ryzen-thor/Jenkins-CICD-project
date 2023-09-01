pipeline { 
	agent any 
	
	environment  {
		IMAGE_TAG = "${BUILD_NUMBER}"
	}

	stages {

	  stage('Checkout') {
	     steps {
	       git branch: 'main', url: 'https://github.com/Ryzen-thor/Jenkins-CICD-project.git'
	      }

	     }

	  stage('Build Docker'){
		steps{
			script{
				sh '''
				echo 'Build Docker Image'
				docker build -t stormbreaker2023/todo-app:${BUILD_NUMBER} .
				'''
			}

		}
		
	  }

	  stage('Push the artifacts'){
		steps{
			
					echo 'push to DockerHub'
     					withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerPass",usernameVariable:"dockerUser")]){
	  			        sh "docker login -u ${env.dockerUser} -p ${env.dockerPass}"
					sh "docker push ${env.dockerUser}/todo-app:${BUILD_NUMBER}"
				
			}
		}
	  }

	  stage('Update k8s manifest and push to repo'){
		steps{
			script{
				withCredentials([usernamePassword(credentialsId:"gitCred",passwordVariable:"gitPass",usernameVariable:"gitUser")]){
				sh '''
				cd deploy
				cat deployment.yaml
				sed -i "s/v2/${BUILD_NUMBER}/g" deployment.yaml
				cat deployment.yaml
				git add deployment.yaml
				git commit -m 'Updated deployment.yaml | using Jenkinsfile pipeline'
				git remote -v
    				
				git push https://github.com/Ryzen-thor/Jenkins-CICD-project.git HEAD:main

				'''
				}
			}
		}
	  }
	 }

}	
