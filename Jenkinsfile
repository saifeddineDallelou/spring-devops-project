pipeline {
    agent any

    tools {
        maven 'Maven3'
        jdk 'jdk17'
    }

    stages {

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
                sh 'docker build -t asgard69/spring-devops:1.0 .'
            }
        }

        stage('Push Docker Image') {
            steps {
                sh 'docker push asgard69/spring-devops:1.0'
            }
        }

    }
}
