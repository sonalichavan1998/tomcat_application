pipeline {
    agent any

    tools {
        maven 'maven 3.9.9'  // Must match name in Jenkins → Global Tool Configuration
    }
 options {
  buildDiscarder logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '4', numToKeepStr: '3')
  timestamps()
}

    environment {
        SONARQUBE_ENV = 'sonar'  // Must match name in Jenkins → Manage Jenkins → Configure System → SonarQube
    }

    stages {
        stage('Cloning Repo') {
            steps {
                git branch: 'main', url: 'https://github.com/sanket0101/tomcat_application.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv("${SONARQUBE_ENV}") {
                    sh 'mvn sonar:sonar'
                }
            }
        }
    }

    post {
        success {
            echo '✅ Pipeline completed successfully!'
        }
        failure {
            echo '❌ Pipeline failed. Check logs.'
        }
    }
}
