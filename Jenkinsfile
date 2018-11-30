pipeline {
    environment {
      DOCKER = credentials('docker-hub')
    }
  agent any
  
  stages {
		stage ('Docker') {
		  steps {
			sh 'docker version'
		  }
		}
  
		stage('checkout') {
			steps {
				git 'https://github.com/andrectw/notepad-test1.git'
			}
		}
        stage("Build") {
            steps {
				script {
					docker.image("openjdk:8-alpine").inside() {
							
							stage("IInit Mysql") {
							  sh 'echo "Tests passed"'
							}
							
						}
				}
            }
        }
    }
}