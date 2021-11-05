pipeline {
    agent {
	docker { image 'adoptopenjdk:11-jdk-hotspot' }
    }
    environment {
                SONAR_CREDENTIALS = credentials('sonar-credentials')
            }
    stages {
        stage('Build') { 
            steps {
                sh './gradlew -PbuildNumber=${BUILD_NUMBER} test assemble'
                echo '$SONAR_CREDENTIALS'
                echo '$SONAR_CREDENTIALS_USR'
                echo '$SONAR_CREDENTIALS_PSW'
            }
        }
    }
}