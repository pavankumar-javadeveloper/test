pipeline {
    agent any

    tools {
        jdk 'JDK21'
        maven 'Maven3'
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                bat 'mvn clean package'
            }
        }
    }

    post {
            success {
                emailext (
                    subject: "✅ Build SUCCESS: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                    body: "Good news! Build completed successfully.",
                    to: "kumarone77@gmail.com"
                )
            }
            failure {
                emailext (
                    subject: "❌ Build FAILED: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                    body: "Build failed. Please check Jenkins logs.",
                    to: "kumarone77@gmail.com"
                )
            }
        }
}
