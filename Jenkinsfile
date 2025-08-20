pipeline {
  agent any
  environment {
    IMAGE_NAME = "demo-ci-cd:latest"
  }
  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }
    stage('Build & Test') {
      steps {
        script {
          // Asigna la ruta de Maven configurada en Jenkins
          def mvnHome = tool name: 'Maven3', type: 'maven'
          bat "\"${mvnHome}\\bin\\mvn\" -B clean package"
        }
      }
    }
    stage('Build Docker Image') {
      steps {
        bat 'docker build -t %IMAGE_NAME% .'
      }
    }
    stage('Run Container') {
      steps {
        bat 'docker rm -f demo-ci-cd || true'
        bat 'docker run -d --name demo-ci-cd -p 8080:8080 %IMAGE_NAME%'
      }
    }
  }
  post {
    always {
      junit '**/target/surefire-reports/*.xml'
      archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
    }
  }
}
