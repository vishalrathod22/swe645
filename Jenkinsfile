pipeline {
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
                echo 'Creating the Jar ...'
                sh 'java -version'
                sh 'jar -cvf swe645.war -C src/main/webapp .'
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
                    myapp.image("${env.BUILD_ID}").push()
                }
            }
        }
        stage("UpdateDeployment") {
            steps {
                sh 'kubectl config view'
                sh 'kubectl get deployments'
                sh "kubectl set image deployment/rancher container-0=vishal77/docker645:${env.BUILD_ID}"
            }
        }
    }
}
