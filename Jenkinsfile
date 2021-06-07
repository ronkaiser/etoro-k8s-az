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
          sh 'curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3'
          sh 'chmod 700 get_helm.sh'
          sh './get_helm.sh'
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