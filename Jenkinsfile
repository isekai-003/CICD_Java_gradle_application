pipeline {
    agent any 
    environment {
        SCANNER_HOME = tool 'sonar'
    }
    stages {
        stage('sonar-quality-check') {
            agent {
                docker {
                    image 'openjdk:17'
                }
            }
            steps {
                script {
                   withSonarQubeEnv('sonar-scanner'){
                   sh ''' 
                   $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=CICD_Java_gradle_application \
                   -Dsonar.java.binaries=. \
                   -Dsonar.projectKey=CICD_Java_gradle_application ''' 
                   }

                     timeout(time: 2, unit: 'MINUTES') {
                      def qg = waitForQualityGate()
                      if (qg.status != 'OK') {
                           error "Pipeline aborted due to quality gate failure: ${qg.status}"
                      }
                    }
                }

            }
        }
    }
}
