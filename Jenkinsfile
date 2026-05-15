pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/ronitkhokharr/8.2CDevSecOps.git'
            }
        }
        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }
        stage('Run Tests') {
            steps {
                bat 'npm test || exit /b 0'
            }
            post {
                always {
                    emailext(
                        to: 'ronitkhokharr@gmail.com',
                        subject: "Run Tests Stage - ${currentBuild.currentResult}: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                        body: """
                            <p>Build Status: <b>${currentBuild.currentResult}</b></p>
                            <p>Job: ${env.JOB_NAME}</p>
                            <p>Build Number: ${env.BUILD_NUMBER}</p>
                            <p>Check console output at: <a href='${env.BUILD_URL}'>${env.BUILD_URL}</a></p>
                        """,
                        mimeType: 'text/html',
                        attachLog: true
                    )
                }
            }
        }
        stage('Generate Coverage Report') {
            steps {
                bat 'npm run coverage || exit /b 0'
            }
        }
        stage('NPM Audit (Security Scan)') {
            steps {
                bat 'npm audit || exit /b 0'
            }
            post {
                always {
                    emailext(
                        to: 'ronitkhokharr@gmail.com',
                        subject: "Security Scan Stage - ${currentBuild.currentResult}: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                        body: """
                            <p>Build Status: <b>${currentBuild.currentResult}</b></p>
                            <p>Job: ${env.JOB_NAME}</p>
                            <p>Build Number: ${env.BUILD_NUMBER}</p>
                            <p>Check console output at: <a href='${env.BUILD_URL}'>${env.BUILD_URL}</a></p>
                        """,
                        mimeType: 'text/html',
                        attachLog: true
                    )
                }
            }
        }
    }
}
