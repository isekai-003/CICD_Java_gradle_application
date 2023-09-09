pipeline {
    agent any 
    tools {
        maven 'maven3'
    }
    environment {
        SCANNER_HOME= tool 'sonar-scanner'
    }
    stages {
        stage('git-checkout') {
            steps {
                git branch: 'classic', url: 'https://github.com/isekai-003/CICD_Java_gradle_application.git'
            }
        }
        stage('git-leaks') {
            steps {
                sh 'rm trufflehog || true'
                sh 'docker run gesellix/trufflehog --json https://github.com/isekai-003/CICD_Java_gradle_application.git > trufflehog'
            }
        }
        stage('Sonarqube') {
            steps {
                withSonarQubeEnv('sonar-server'){
                   sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=CICD_Java_gradle_application \
                   -Dsonar.java.binaries=. \
                   -Dsonar.projectKey=CICD_Java_gradle_application '''
               }
               timeout(time: 5, unit: 'MINUTES') {
                      def qg = waitForQualityGate()
                      if (qg.status != 'OK') {
                           error "Pipeline aborted due to quality gate failure: ${qg.status}"
                      }
                    }
            }
        }
        stage(build-stage) {
            steps {
                sh 'mvn clean install'
            }
        }
        
    }
}