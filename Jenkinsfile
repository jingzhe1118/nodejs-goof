pipeline {
    agent any

    tools {
        nodejs 'NodeJS 18'  // 确保已在 Jenkins 全局工具配置中设置
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

        stage('Snyk Auth') {
            steps {
                withEnv(["SNYK_TOKEN=${SNYK_TOKEN}"]) {
                    bat 'npx snyk auth %SNYK_TOKEN%'
                }
            }
        }

        stage('Test with Snyk') {
            steps {
                withEnv(["SNYK_TOKEN=${SNYK_TOKEN}"]) {
                    bat """
                        echo Running Snyk test with token: %SNYK_TOKEN%
                        npx snyk test || exit 0
                    """
                }
            }
        }

        stage('SonarCloud Analysis') {
            steps {
                withSonarQubeEnv('SonarCloud') {
                    bat """
                        npx sonar-scanner ^
                        -Dsonar.projectKey=jingzhe1118_nodejs-goof ^
                        -Dsonar.organization=jingzhe1118 ^
                        -Dsonar.host.url=https://sonarcloud.io ^
                        -Dsonar.login=%SONAR_TOKEN%
                    """
                }
            }
        }
    }
}
