pipeline {
    agent any
    environment {
        PROJECT_ID = 'rancher'
        CLUSTER_NAME = 'rancher'
        LOCATION = 'us-east-1c'
        DOCKERHUB_PASS = 'Vanitha%12'
    }
    stages {
        stage("Checkout code") {
            steps {
                checkout scm
            }
        }
        stage('BuildWAR') {
            steps {
                echo 'Creating the Jar ...'
                sh 'java -version'
                sh 'rm -rf *.war'
                sh 'jar -cvf swe645.war -C src/main/webapps .'
                sh 'echo $DOCKERHUB_PASS | docker login -u vishal77 --password-stdin'
			sh 'docker build -t vishal77/swe645 .'
		// script {
  //                   // Login to Docker with secure password handling
  //                   withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'DOCKERHUB_USER', passwordVariable: 'DOCKERHUB_PASS')]) {
  //                       sh 'echo $DOCKERHUB_PASS | docker login -u $DOCKERHUB_USER --password-stdin'
  //                   }   
				
            }
        }
		stage("Pushing image to docker"){
			steps{
				script{
					sh 'docker push vishal77/swe645'
				}
			}
		}
    
        stage("UpdateDeployment") {
            steps {
					sh 'kubectl rollout restart deploy swe645-deploy'
	    }
        }
    }
}

