pipeline {
  agent any

    stages {
      // Upgrade helm chart
      stage('Upgrade helm') {
        steps {
          sh 'helm upgrade ./simple-web/ simple-web'
        }
      }
    }
  }