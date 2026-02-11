pipeline {

    agent any

    tools {
        maven 'Maven3'      // your configured Maven name
        jdk 'JDK21'         // your configured JDK name
    }

    options {
        timestamps()
        disableConcurrentBuilds()
    }

    environment {
        EMAIL_RECIPIENT = "kumarone77@gmail.com"
        APP_NAME = "springboot-app"
    }

    stages {

        stage('Checkout Source') {
            steps {
                echo 'Checking out code from SCM...'
                checkout scm
            }
        }

        stage('Verify Tools') {
            steps {
                echo 'Java Version:'
                bat 'java -version'

                echo 'Maven Version:'
                bat 'mvn -version'
            }
        }

        stage('Clean') {
            steps {
                echo 'Cleaning workspace...'
                bat 'mvn clean'
            }
        }

        stage('Compile') {
            steps {
                echo 'Compiling project...'
                bat 'mvn compile'
            }
        }

        stage('Unit Tests') {
            steps {
                echo 'Running tests...'
                bat 'mvn test'
            }
        }

        stage('Package Artifact') {
            steps {
                echo 'Packaging JAR...'
                bat 'mvn package -DskipTests'
            }
        }

        stage('Archive Artifact') {
            steps {
                echo 'Archiving JAR into Jenkins...'
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
    }

    post {

        success {
            emailext(
                subject: "✅ SUCCESS: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: """
                Build Successful 🎉

                Application: ${APP_NAME}
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

                Application: ${APP_NAME}
                Job: ${env.JOB_NAME}
                Build Number: ${env.BUILD_NUMBER}

                Check Logs:
                ${env.BUILD_URL}
                """,
                to: "${EMAIL_RECIPIENT}"
            )
        }

        always {
            echo "Pipeline finished at ${new Date()}"
        }
    }
}
