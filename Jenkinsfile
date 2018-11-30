pipeline {
    environment {
      DOCKER = credentials('docker-hub')
    }
  agent any
  
  stages {
        stage('Build') {
            steps {
                docker.image(openjdk:8-alpine).inside {
						git 'https://github.com/andrectw/notepad-test1.git'
						sh "docker container run -d --name mysql -e MYSQL_DATABASE=notepad -e MYSQL_ROOT_PASSWORD=root -p 3306:3306 mysql:5.7"
				
						sh "mvn clean verify"
				
						sh "docker stop mysql && docker rm mysql"
					}
            }
        }
    }
}