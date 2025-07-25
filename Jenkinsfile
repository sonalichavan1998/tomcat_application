pipeline {
    agent any

    tools {
        maven 'maven 3.9.10' // Ensure this is configured under Jenkins Global Tool Configuration
    }

    stages {

        stage('Git Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/sonalichavan1998/tomcat_application.git'
            }
        }

        stage('Maven Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Sonar Analysis') {
            steps {
                sh 'mvn sonar:sonar'
            }
        }

        stage('Nexus Upload') {
            steps {
                sh 'mvn deploy'
            }
        }

        stage('Tomcat Hosting') {
            steps {
                sshagent(['tomcat']) {
                    sh """
                        scp -o StrictHostKeyChecking=no target/petclinic.war ubuntu@13.203.213.94:/opt/apache-tomcat-9.0.106/webapps/
                    """
                }
            }
        }

    } // end stages
 post {
          success {
              slackSend (
                  color: 'good', // green
                  message: "✅ Build Success - Job: ${env.JOB_NAME}, Build: #${env.BUILD_NUMBER}\n<${env.BUILD_URL}|Click here to view details>"
              )
          }
          failure {
              slackSend (
                  color: 'danger', // red
                  message: "❌ Build Failed - Job: ${env.JOB_NAME}, Build: #${env.BUILD_NUMBER}\n<${env.BUILD_URL}|Click here to view details>"
              )
          }
          unstable {
              slackSend (
                  color: 'warning', // yellow
                  message: "⚠️ Build Unstable - Job: ${env.JOB_NAME}, Build: #${env.BUILD_NUMBER}\n<${env.BUILD_URL}|Click here to view details>"
              )
          }
      }	  
  
  
} // end pipeline
