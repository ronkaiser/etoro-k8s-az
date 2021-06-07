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

      // Verify helm installation
      stage('verify helm') {
        steps {
          sh 'chmod 700 ./install_helm.sh'
          sh './install_helm.sh'
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