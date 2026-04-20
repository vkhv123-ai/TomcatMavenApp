pipeline {
    agent any

    environment {
        TOMCAT_URL = 'http://13.60.79.117:8080'
        WAR_FILE = 'target/*.war'
    }

    tools {
        maven 'Maven-3.8'
        jdk 'Java-21'
    }

    stages {

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
                    url: env.TOMCAT_URL
                )],
                contextPath: '/TomcatMavenApp',
                war: env.WAR_FILE
            }
        }

        stage('Smoke Test') {
            steps {
                sh 'curl ${TOMCAT_URL}/TomcatMavenApp'
            }
        }
    }
}
