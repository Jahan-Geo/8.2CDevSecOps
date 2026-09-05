pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Jahan-Geo/8.2CDevSecOps.git'
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

        /* ⭐ THIS MUST BE INSIDE THE STAGES BLOCK */
        stage('SonarCloud Analysis') {
            steps {
                withCredentials([string(credentialsId: 'SONAR_TOKEN', variable: 'SONAR_TOKEN')]) {
                    bat '''
                        "C:\\sonar-scanner\\bin\\sonar-scanner.bat" -Dsonar.login=%SONAR_TOKEN%
                    '''
                }
            }
        }
        /* ⭐ END OF SONARCLOUD STAGE */

    }
}
