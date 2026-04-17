pipeline {
    agent any

    environment {
        TOMCAT_URL = 'http://3.91.98.43:8080'
        WAR_FILE = 'target/*.war'
    }

    tools {
        maven 'Maven-3.8'
        jdk 'Java-11'
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                url: 'https://github.com/vkhv123-ai/TomcatMavenApp.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Unit Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Archive') {
            steps {
                archiveArtifacts artifacts: 'target/*.war'
            }
        }

        stage('Deploy') {
            steps {
                deploy adapters: [tomcat9(
                    credentialsId: 'tomcat-deployer-creds',
                    url: 'http://3.91.98.43:8080'
                )],
                contextPath: '/TomcatMavenApp',
                war: 'target/*.war'
            }
        }

        stage('Smoke Test') {
            steps {
                sh 'curl http://3.91.98.43:8080/TomcatMavenApp'
            }
        }
    }
}
