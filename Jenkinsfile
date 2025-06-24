pipeline {
  agent any

  environment {
    IMAGE_NAME = "myapp:latest"
    TMPDIR = "${env.WORKSPACE}/trivy-tmp"
  }

  stages {
    stage('Checkout Code') {
      steps {
        git branch: 'master', url: 'https://github.com/Anandb007/java-trivy-practice.git'
      }
    }

    stage('Build Java App') {
      steps {
        sh 'mvn clean package'
      }
    }

    stage('Build Docker Image') {
      steps {
        sh 'docker build -t $IMAGE_NAME .'
      }
    }

    stage('Scan with Trivy') {
      steps {
        sh 'trivy image -f json -o trivy-report.json $IMAGE_NAME'
      }
    }
  }

  post {
    always {
      cleanWs()
    }
  }
}
