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
                sh 'docker login -u vishal77 -p ${DOCKERHUB_PASS}'
				sh 'docker build -t vishal77/swe645 .'
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
