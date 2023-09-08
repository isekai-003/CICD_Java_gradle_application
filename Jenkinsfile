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
               withSonarQubeEnv('sonar-scanner') {
                 sh ''' $SCANNER_HOME/bin/sonar -Dsonar.projectName=CICD_Java_gradle_application \
                   -Dsonar.java.binaries=. \
                   -Dsonar.projectKey=CICD_Java_gradle_application '''
    
}

            }
        }
    }
}
