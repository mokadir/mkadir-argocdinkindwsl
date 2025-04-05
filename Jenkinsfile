pipeline {
	agent any
	
	tools {
		maven 'maven3'
	
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
				git branch: 'dev', credentialsId: 'git-cred', url: 'https://github.com/mokadir/mkadir-argocdinkindwsl.git'
			}
		}

		stage ('Compile'){
			steps {
                echo "****** Compile running....******"
				sh "mvn compile"
			}
		}
		
		stage ('Build Application'){
			steps {
                echo "****** Build Application running....******"
				sh "mvn clean package -DskipTests=true"
			}
		}
		
/*  	stage('Code Coverage ') {
			steps {
				echo "****** Code Coverage running....******"
				echo "Running Code Coverage ..."
				sh "mvn jacoco:report"
			} 
		} */
				
    	stage ('Unit Test'){
			steps {
				echo "****** Unit Test running....******"
				sh "mvn test -DskipTests=true" 
			}
		} 

		
/*  	stage ('File System Scan'){
			steps {
				echo "****** File System scan running....******"
				sh "trivy fs --format table -o trivyscanfs.html ."
			}
		}  */

		stage ('Docker Build & Tag'){
			steps {
				script {
                    echo "****** Docker Build and Tag Image running....******"
					withDockerRegistry(credentialsId: 'docer-cred') {
						sh "docker build -t mokadir/mkadir-registerapp:latest ."
					}
				}
			}
		}
	
/* 		stage ('Docker Image Scan'){ 			
			steps {
                echo "****** Docker Image Scan by Trivy running....******"
				sh "trivy image --scanners vuln --format table -o trivyscandocr.html mokadir/mkadir-registerapp:latest"
			}
		}  */ //need lot of ram
		
		stage ('Docker Push'){
			steps {
				script {
                    echo "****** Docker Push Image running....******"
					withDockerRegistry(credentialsId: 'docer-cred') {
						sh "docker push mokadir/mkadir-registerapp:latest"
					}
				}
			}
		}
		
/*  	stage('Smoke Test') {
			steps { 
				echo "****** Smoke Test Image running....******"
				sh "docker run -d --name smokerun -p 8080:8080 mokadir/mkadir-registerapp:latest"
				sh "sleep 90"
				sh "docker rm --force smokerun"
			}
		}  */
		
/* 		stage ('Deploy to Kubernetes'){
    	} */		
	}
}