pipeline {
    agent any

    tools {
        nodejs 'NodeJS 18'  // 确保你已在 Jenkins -> Global Tool Configuration 中配置该名称
    }

    environment {
        SONAR_TOKEN = credentials('SONAR_TOKEN')
        SNYK_TOKEN = credentials('SNYK_TOKEN')
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/jingzhe1118/nodejs-goof.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }

        stage('Test') {
            steps {
                withEnv(["SNYK_TOKEN=${SNYK_TOKEN}"]) {
                    bat """
                        echo Running Snyk test with token: %SNYK_TOKEN%
                        npx snyk test
                    """
                }
            }
        }

        stage('SonarCloud Analysis') {
            steps {
                withSonarQubeEnv('SonarCloud') {
                    bat 'npx sonar-scanner'
                }
            }
        }
    }
}
