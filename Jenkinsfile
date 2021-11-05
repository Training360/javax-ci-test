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
                // sh './gradlew -PbuildNumber=${BUILD_NUMBER} test assemble'
		withCredentials([usernamePassword(credentialsId: 'sonar-credentials', passwordVariable: 'pass', usernameVariable: 'user')]) {
//		  echo $user
//                  echo $pass
		  sh 'echo ${user}'
                  sh 'echo ${pass}'
		}
		sh 'echo ${SONAR_CREDENTIALS} blahblah ${SONAR_CREDENTIALS_USR} blahblah ${SONAR_CREDENTIALS_PSW}'
		echo '${SONAR_CREDENTIALS}'
            }
        }
    }
}