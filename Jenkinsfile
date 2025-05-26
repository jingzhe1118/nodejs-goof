pipeline {
  agent any

  tools {
    nodejs 'NodeJS 18'
  }

  environment {
    SONAR_TOKEN = credentials('SONAR_TOKEN')
  }

  stages {
    stage('Checkout') {
      steps {
        git 'https://github.com/jingzhe1118/nodejs-goof.git'
      }
    }

    stage('Install Dependencies') {
      steps {
        sh 'npm install'
      }
    }

    stage('Test') {
      steps {
        sh 'npm test'
      }
    }

    stage('SonarCloud Analysis') {
      steps {
        withSonarQubeEnv('SonarCloud') {
          sh 'npx sonar-scanner'
        }
      }
    }
  }
}
