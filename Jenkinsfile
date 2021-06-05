pipeline {
  agent any

    stages {
      // Upgrade helm chart
      stage('Chart check') {
        steps {
          sh 'helm lint ./simple-web'
        }
      }
    }
  }