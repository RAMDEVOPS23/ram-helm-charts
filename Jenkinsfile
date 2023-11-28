pipeline {
    agent any

    environment {
        registry = "308954559815.dkr.ecr.us-east-1.amazonaws.com/my-docker-repo"
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/mmbabu1988/helm-spring-boot.git']])
            }
        }
        
        stage ("Build JAR") {
            steps {
                sh "mvn clean install"
            }
        }
        
        stage ("Build Image") {
            steps {
                script {
                    docker.build registry
                }
            }
        }
        
        stage ("Push to ECR") {
            steps {
                script {
                    sh "aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 308954559815.dkr.ecr.us-east-1.amazonaws.com"
                    sh "docker push 308954559815.dkr.ecr.us-east-1.amazonaws.com/my-docker-repo:latest"
                    
                }
            }
        }
        	
		stage("Helm Deploy") {
		    steps {
			    script {
				    sh "helm upgrade first --install mychart --namespace helm-deployment --set image.tag=latest"
				}
			}
		}	
    }
}
