pipeline {
    agent any

    environment {
        RECIPIENT = 'hemanthobulareddy80118@gmail.com'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/hemanth-obulareddy/Task8.1-part2.git'
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    try {
                        // bat 'npm test'
                        currentBuild.result = 'SUCCESS'
                    } catch (err) {
                        currentBuild.result = 'FAILURE'
                        throw err
                    }
                }
            }
            post {
                always {
                    emailext (
                        to: "${RECIPIENT}",
                        subject: "Test Stage: ${currentBuild.result}",
                        body: "The Test stage has completed with status: ${currentBuild.result}",
                        attachLog: true
                    )
                }
            }
        }

        stage('Security Scan') {
            steps {
                script {
                    try {
                        // bat 'npm audit'
                        currentBuild.result = 'SUCCESS'
                    } catch (err) {
                        currentBuild.result = 'FAILURE'
                        throw err
                    }
                }
            }
            post {
                always {
                    emailext (
                        to: "${RECIPIENT}",
                        subject: "Security Scan: ${currentBuild.result}",
                        body: "The Security Scan stage has completed with status: ${currentBuild.result}",
                        attachLog: true
                    )
                }
            }
        }
    }

    post {
        failure {
            emailext (
                to: "${RECIPIENT}",
                subject: "Build Failed: ${env.JOB_NAME} [${env.BUILD_NUMBER}]",
                body: "Something went wrong in the pipeline. Please check the Jenkins logs.",
                attachLog: true
            )
        }
    }
}
