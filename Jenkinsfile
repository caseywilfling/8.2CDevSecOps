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
                    wget -q https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-7.2.0.5079-linux-x64.zip -O sonar-scanner.zip
                    unzip -o -q sonar-scanner.zip
                    export PATH=$PWD/sonar-scanner-7.2.0.5079-linux-x64/bin:$PATH

                    # Run SonarScanner with inline config
                    sonar-scanner \
                      -Dsonar.projectKey=caseywilfling_8.2CDevSecOps \
                      -Dsonar.organization=caseywilfling \
                      -Dsonar.host.url=https://sonarcloud.io \
                      -Dsonar.sources=. \
                      -Dsonar.exclusions=node_modules/**,test/** \
                      -Dsonar.javascript.lcov.reportPaths=coverage/lcov.info \
                      -Dsonar.projectName="NodeJS Goof Vulnerable App" \
                      -Dsonar.sourceEncoding=UTF-8 \
                      -Dsonar.login=$SONAR_TOKEN
                '''
                }
            }
        }
    }
}





