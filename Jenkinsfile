pipeline {
  agent any

  environment {
    IMAGE_NAME = "myapp:latest"
    TMPDIR = "${env.WORKSPACE}/trivy-tmp"
  }

  stages {
    stage('Checkout Code') {
      steps {
        git branch: 'master', url: 'https://github.com/YourUser/java-practice'
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

    stage('Convert to HTML') {
      steps {
        sh 'curl -L -o html.tpl https://raw.githubusercontent.com/aquasecurity/trivy/main/contrib/html.tpl'
        sh 'trivy image --format template --template "@html.tpl" -o trivy-report.html $IMAGE_NAME'
      }
    }

    stage('Archive HTML Report') {
      steps {
        archiveArtifacts artifacts: 'trivy-report.html', fingerprint: true
      }
    }
  }

  post {
    always {
      cleanWs()
    }
  }
}
