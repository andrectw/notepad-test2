
def dockerMysqlIP = 'UNKNOWN'

pipeline {
    environment {
      DOCKER = credentials('docker-hub')
    }
  agent any
  
  tools {
    maven 'M3'
  }
  
  stages {
		stage ('Docker') {
		  steps {
			sh 'docker version'
		  }
		}
  
		stage('checkout') {
			steps {
				git 'https://github.com/andrectw/notepad-test2.git'
			}
		}
		
        stage("Build") {
            steps {
				 sh 'mvn -B -DskipTests clean package'
            }
        }		
		stage("Test") {
		
            steps {
				
				script {
					dockerMysqlIP = sh (
						script: 'docker inspect -f "{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}" mysql',
						returnStdout: true
					)
				}
		
				sh "mvn clean verify -DENV_TEST_MYSQL_HOST=${dockerMysqlIP}"
		
				sh "docker stop mysql && docker rm mysql"
            }
        }
    }
}