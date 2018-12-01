pipeline {
    environment {
      DOCKER = credentials('docker-hub')
    }
  agent any
  
  tools {
    maven 'M3'
  }
  
  def dockerMysqlIP
  
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
				 sh 'mvn -B -DskipTests clean package'
            }
        }		
		stage("Test") {
            steps {
				
				dockerMysqlIP = sh (
					script: 'docker inspect -f "{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}" mysql',
					returnStdout: true
				)
		
				sh "mvn clean verify -DENV_TEST_MYSQL_HOST=${dockerMysqlIP}"
		
				sh "docker stop mysql && docker rm mysql"
            }
        }
    }
}