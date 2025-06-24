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
        dir('myapp') {
          sh 'mvn clean package'
        }
      }
    }

    stage('Build Docker Image') {
      steps {
        dir('myapp') {
          // Show the structure for debugging
         // sh 'ls -l target/'
          // Build the Docker image using the local Dockerfile
          sh 'docker build -t $IMAGE_NAME .'
        }
      }
    }

    stage('Scan with Trivy') {
      steps {
        dir('myapp') {
          sh 'mkdir -p $TMPDIR'
          sh 'export TMPDIR=$TMPDIR && trivy image -f json -o trivy-report.json $IMAGE_NAME'
        }
      }
    }

    stage('Archive Trivy Report') {
      steps {
        dir('myapp') {
          archiveArtifacts artifacts: 'trivy-report.json', fingerprint: true
        }
      }
    }
  }

  post {
    always {
      cleanWs()
    }
  }
}

