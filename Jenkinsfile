pipeline {
  agent any

    stages {

      // Check chart syntax
      stage('Chart check') {
        steps {
          sh 'helm lint ./simple-web'
        }
      }

      // Connect to AKS cluster
      stage('AKS authentication') {
        steps {
          sh 'az login --identity'
          sh 'az aks get-credentials --resource-group devops-interview-rg --name ron-interview-aks --admin'
        }
      }

      // Upgrade helm chart
      stage('Chart upgrade') {
        steps {
          sh 'helm upgrade simple-web ./simple-web/'
        }
      }
    }
  }