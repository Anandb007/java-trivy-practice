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
          sh 'cp target/myapp-1.0-SNAPSHOT.jar target/myapp.jar'  // rename
        }
      }
    }

    stage('Build Docker Image') {
      steps {
        dir('myapp') {
          sh 'ls -lh target/'
          sh 'test -f target/myapp.jar || (echo "JAR file not found!" && exit 1)'
          sh 'docker build -t $IMAGE_NAME .'
        }
      }
    }

    stage('Scan with Trivy') {
      steps {
        dir('myapp') {
          sh '''
            mkdir -p $TMPDIR
            echo "Scanning with Trivy..."
            trivy image --no-progress $IMAGE_NAME > trivy-report.txt

            echo "<html><body><h2>Trivy Vulnerability Report</h2><pre>" > trivy-report.html
            cat trivy-report.txt >> trivy-report.html
            echo "</pre></body></html>" >> trivy-report.html
          '''
        }
      }
    }

    stage('Archive Trivy Report') {
      steps {
        dir('myapp') {
          archiveArtifacts artifacts: 'trivy-report.html', fingerprint: true
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

