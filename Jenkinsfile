pipeline {
    agent any 
    tools {
        gradle 'gradle'
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
                    withSonarQubeEnv( 'sonar-server') {
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
