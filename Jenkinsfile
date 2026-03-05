pipeline {
    agent any

    tools {
        maven 'Maven3'
        jdk 'jdk17'
    }

    stages {

	stage('Clean workspace') {
	  steps {
	    cleanWs()
	  }
	}
        stage('Clone Repository') {
            steps {
                git branch: 'main',
                    credentialsId: 'github-token',
                    url: 'https://github.com/Asgard69/spring-devops-project.git'
            }
        }

        stage('Compile Project') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh 'mvn sonar:sonar'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t dallelou/spring-devops:1.0 .'
            }
        }

        stage('Push Docker Image') {
            steps {
                sh 'docker push dallelou/spring-devops:1.0'
            }
        }
	stage('Run MySQL and Spring App') {
 	   steps {
       	        sh '''
         	 # Stop / remove old containers if they exist
         	 docker rm -f mysql || true
         	 docker rm -f student-app || true

         	 # Start MySQL
         	 docker run -d --name mysql \
           	 -e MYSQL_ROOT_PASSWORD=Root@123 \
           	 -e MYSQL_DATABASE=studentdb \
           	 -p 3306:3306 \
           	 mysql:latest

	         # Start Spring Boot app, linked to MySQL
         	 docker run -d --name student-app \
           	 --link mysql:mysql \
           	 -e SPRING_DATASOURCE_URL=jdbc:mysql://mysql:3306/studentdb \
           	 -e SPRING_DATASOURCE_USERNAME=root \
           	 -e SPRING_DATASOURCE_PASSWORD=Root@123 \
           	 -p 8081:8080 \
           	 dallelou/spring-devops:1.0
       		 '''
   		 }
	}
	


    }
}
