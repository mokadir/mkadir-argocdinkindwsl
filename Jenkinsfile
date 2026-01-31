pipeline {
	agent {
		kubernetes {
			yaml '''
				apiVersion: v1
				kind: Pod
				spec:
				containers:
				- name: docker
					image: docker:latest
					command:
					- cat
					tty: true
					volumeMounts:
					- mountPath: /var/run/docker.sock
					name: docker-sock
				volumes:
				- name: docker-sock
					hostPath:
					path: /var/run/docker.sock    
				'''
		}
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
					container('docker') {
          					sh 'docker build -t mokadir/mkadir-cafeapp:1 .'
        			}
				}
			}
		}
	
		stage ('Docker Push'){
			steps {
				script {
                    echo "****** Docker Push Image running....******"
					withDockerRegistry(credentialsId: 'dockerhub-cred') {
						container('docker') {
							sh "docker push mokadir/mkadir-cafeapp:1"
						}
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