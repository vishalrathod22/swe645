pipeline {
	agent {
        docker {
            image 'vishal77/docker645:latest'
            args '-v /var/run/docker.sock:/var/run/docker.sock'
        }
    }
    agent any
    environment {
        PROJECT_ID = 'rancher'
        CLUSTER_NAME = 'rancher'
        LOCATION = 'us-east-1a'
    }
    stages {
        stage("Checkout code") {
            steps {
                checkout scm
            }
        }
        stage('BuildWAR') {
            steps {
            
            	// dir('src/main/webapp') {
            		echo 'Creating the Jar ...'
					sh 'java -version'
					sh 'jar -cvf swe645.war *'
            	// }
            }
        }
        
        stage("Build image") {
            steps {
                script {
                    myapp = docker.build("vishal77/docker645:${env.BUILD_ID}")
                }
            }
        }
        stage("Push image") {
            steps {
                script {
                	sh 'docker login -u vishal77 -p Vanitha%12'
					myapp.push("${env.BUILD_ID}")
                }
            }
        }        
        stage("UpdateDeployment") {
			steps{
				sh 'kubectl config view'
				sh "kubectl get deployments"
				sh "kubectl set image deployment/swe645deployment container-0=vishal77/docker645:${env.BUILD_ID}"
			}
		}
    }    
}
