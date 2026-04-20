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
                sh '''
                export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64
                export PATH=$JAVA_HOME/bin:$PATH
                mvn clean package -DskipTests
                '''
            }
        }

        stage('Unit Test') {
            steps {
                sh '''
                export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64
                export PATH=$JAVA_HOME/bin:$PATH
                mvn test
                '''
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


/*
This Jenkins pipeline automates the CI/CD process for a Maven-based Java web application.
The pipeline starts by building the application using Maven and packaging it into a WAR file.
It then runs unit tests to validate the application functionality.
The generated artifact is archived for future reference.
Next, the application is deployed to an Apache Tomcat server using configured credentials.
Finally, a smoke test is performed to verify that the application is successfully running.
*/
