pipeline {
    agent any

    environment {
        SONAR_TOKEN = credentials('SONAR_TOKEN')
    }

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
        }
        stage('SonarCloud Analysis') {
            steps {
                bat '''
                    curl -sSLo sonar-scanner.zip https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-5.0.1.3006-windows.zip
                    tar -xf sonar-scanner.zip
                    sonar-scanner-5.0.1.3006-windows\\bin\\sonar-scanner.bat -Dsonar.projectKey=ronitkhokharr_8.2CDevSecOps -Dsonar.organization=ronitkhokharr -Dsonar.host.url=https://sonarcloud.io -Dsonar.login=%SONAR_TOKEN%
                '''
            }
        }
    }
}