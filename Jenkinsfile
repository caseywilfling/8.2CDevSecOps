pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Clone the repository from GitHub
                git branch: 'main', url: 'https://github.com/caseywilfling/8.2CDevSecOps.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                // Install required npm packages
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                // Run tests; continue pipeline even if tests fail
                sh 'npm test || true'
            }
        }

        stage('Generate Coverage Report') {
            steps {
                // Generate code coverage report; allow failure
                sh 'npm run coverage || true'
            }
        }

        stage('NPM Audit (Security Scan)') {
            steps {
                // Run security audit to find known vulnerabilities
                sh 'npm audit || true'
            }
        }
    }
}
