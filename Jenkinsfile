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
							  sh "docker container run -d --name mysql -e MYSQL_DATABASE=notepad -e MYSQL_ROOT_PASSWORD=root -p 3306:3306 mysql:5.7"
							}
							
							stage("run tests") {
							  sh "mvn clean verify"
							}
							
							stage ("stop mysql") {
								sh "docker stop mysql && docker rm mysql"
							}
					
						}
				}
            }
        }
    }
}