pipeline {
	agent any

	tools{
		DockerTool 'dockerlatest'
	}
	
	stages {
		stage ('Clean Workspace'){
			steps {
                echo "****** Workspace Cleanup running....******"
				cleanWs()
			}
		}
	
		stage ('Git Checkout'){
			steps {
                echo "****** Git Checkout running....******"
				git branch: 'dev', credentialsId: 'github-cred', url: 'https://github.com/mokadir/cafeapp.git'
			}
		}
/*		This app do not need compilation. its already compiled and ready to run. so no need to build it again. */
		
		
		stage ('Docker Build & Tag'){
			steps {
				script {
                    echo "****** Docker Build and Tag Image running....******"
					withDockerRegistry(credentialsId: 'docerhub-cred') {
						sh "docker build -t mokadir/mkadir-cafeapp:1 ."
					}
				}
			}
		}
	
		stage ('Docker Push'){
			steps {
				script {
                    echo "****** Docker Push Image running....******"
					withDockerRegistry(credentialsId: 'docerhub-cred') {
						sh "docker push mokadir/mkadir-cafeapp:1"
					}
				}
			}
		}

		stage('Trigger Deployment'){
			steps { 
			   script {
                    echo "****** Deployment running.... ******"
					echo "Next: Trigger CD Pipeline ......" 
				}		
			}
		}
    	}		
}