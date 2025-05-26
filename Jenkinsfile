pipeline {
    agent any

    tools {
        nodejs 'NodeJS 18'
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
            environment {
                SNYK_TOKEN = "${env.SNYK_TOKEN}"
            }
            steps {
                bat 'npx snyk auth --token %SNYK_TOKEN%'
                bat 'npx snyk test'
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
