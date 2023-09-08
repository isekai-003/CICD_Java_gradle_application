pipeline {
    agent any 
    environment {
        SCANNER_HOME = tool ''
    }
    stages {
        stage('sonar-quality-check') {
            agent {
                docker {
                    image 'openjdk:11'
                }
            }
            steps {
                script {
                    withSonarQubeEnv(credentialsId: 'sonar') {
                        sh 'chmod +x gradlew'
                        sh './gradlew sonarqube'
                    }
                     timeout(time: 5, unit: 'MINUTES') {
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
