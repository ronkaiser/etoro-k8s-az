pipeline {
  agent any

    stages {

      // Check chart syntax
      stage('Chart check') {
        steps {
          sh 'helm lint ./simple-web'
        }
      }

      // Upgrade helm chart
      stage('Chart upgrade') {
        steps {
          sh 'helm lint ./simple-web'
        }
      }
    }
  }