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
			
				sh """
    				    cd deploy
    				    sed -i "s/v2/${BUILD_NUMBER}/g" deployment.yaml
		                    git config --global user.name "Ryzen-thor"
		                    git config --global user.email "aman.friend2016@gmail.com"
		      	   	   
		                    git add deployment.yaml
		                    git commit -m "Updated Deployment Manifest"
		      		    git remote set-url origin https://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@github.com/Jenkins-CICD-project.git
				    git push origin main
		                """
				// withCredentials([gitUsernamePassword(credentialsId: 'gitCred', passwordVariable:'gitPass',usernameVariable:'gitUser')]) {
                    		// sh "git push aman main"
                // }
				
				
			
		
	  }
	 }

}
}
