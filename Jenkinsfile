pipeline {
    agent any

    tools {
        maven 'Maven3'     // Name you configured in Manage Jenkins → Tools
        jdk 'JDK21'       // Your JDK name in Jenkins tools
    }

    environment {
        EMAIL_RECIPIENT = "kumarone77@gmail.com"
    }

    stages {

        stage('Checkout') {
            steps {
                echo 'Cloning source code...'
                checkout scm
            }
        }

        stage('Clean') {
            steps {
                echo 'Cleaning project...'
                bat 'mvn clean'
            }
        }

        stage('Build') {
            steps {
                echo 'Building project...'
                bat 'mvn compile'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                bat 'mvn test'
            }
        }

        stage('Package') {
            steps {
                echo 'Packaging JAR...'
                bat 'mvn package -DskipTests'
            }
        }
    }

    post {

        success {
            emailext(
                subject: "✅ SUCCESS: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: """
                Build Successful 🎉

                Job: ${env.JOB_NAME}
                Build Number: ${env.BUILD_NUMBER}
                URL: ${env.BUILD_URL}
                """,
                to: "${EMAIL_RECIPIENT}"
            )
        }

        failure {
            emailext(
                subject: "❌ FAILED: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: """
                Build Failed ⚠️

                Job: ${env.JOB_NAME}
                Build Number: ${env.BUILD_NUMBER}
                Check Logs: ${env.BUILD_URL}
                """,
                to: "${EMAIL_RECIPIENT}"
            )
        }

        always {
            echo 'Pipeline finished.'
        }
    }
}
