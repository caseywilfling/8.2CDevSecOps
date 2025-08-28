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

        stage('SonarCloud Analysis') {
            steps {
                withCredentials([string(credentialsId: 'SONAR_TOKEN', variable: 'SONAR_TOKEN')]) {
                    sh '''
                    # Download and unzip SonarScanner CLI
                    wget https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-7.2.0.5079-linux-x64.zip -O sonar-scanner.zip
                    unzip -q sonar-scanner.zip
                    export PATH=$PWD/sonar-scanner-*/bin:$PATH

                    # Run SonarScanner using properties file
                    sonar-scanner -Dsonar.login=$SONAR_TOKEN
                    '''
                }
            }
        }
    }
}

