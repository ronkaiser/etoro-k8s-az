# etoro-k8s-az
eToro Task - Kubernetes on Azure Cloud

## Description
In this task we are going to use AKS cluster in order to deploy our simple-app.
By creating helm chart we are going to provide values in order to create our infrastructure.
helm will be also configured to manage horizontal pod autoscaling.
In additional we are going to install Jenkins and create pipeline which will deploy helm chart from our repo.

### AKS details
AKS name: ron-interview-aks  
Resource group: devops-interview-rg

### Image
Image: simple-web  
Registry: acrinterview.azurecr.io

## Preparation
- Download SSH key, change permissions and connect to Linux VM
- Connect to VM and install `Azure CLI`, `kubectl`, `helm` and `jenkins`

## Ingress nginx controller installation
- Login to Azure cloud by **managed identity**  
  `az login --identity`
- Connect to AKS cluster as admin  
  `az aks get-credentials --resource-group devops-interview-rg --name ron-interview-aks --admin`
- Installation of ingress controller using Helm:
  ```
  helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
  helm repo update
  helm install ingress-nginx ingress-nginx/ingress-nginx
  ```
## Create helm chart
- Create helm chart tree by the following command:  
  `helm create simple-web`
- Replace values in values file  
  **values changes:**
  - image: *repository* and *tag*  
  - disable *serviceAccount*  
  - ingress enable, remove *host* value, change *pathType*  
  - resources: *cpu* - limits and requests  
  - autoscaling enable: *minReplicas*, *maxReplicas* and *targetCPUUtilizationPercentage*
- Check before deploy  
  `helm install --dry-run --debug --generate-name ./simple-web/`
- Deploy the chart by the following command:  
  `helm install simple-web ./simple-web/`  


### Useful commands:
**autoscaling check**  
`kubectl run -i --tty load-generator --rm --image=busybox --restart=Never -- /bin/sh -c "while sleep 0.01; do wget -q -O- http://[ip]; done"`  

**verify deployment before install**  
`helm install --dry-run --debug --generate-name ./simple-web/`  

**helm install**  
`helm install simple-web ./simple-web/`

**helm upgrade**  
`helm upgrade simple-web ./simple-web/`

**delete helm**  
`helm delete simple-web`

## Jenkins Pipeline
The pipeline doing the following:  
- verify helm chart syntax using lint command  
- Authenticate with Azure and AKS cluster by running the following commands:  
  ```
  az login --identity  
  az aks get-credentials --resource-group devops-interview-rg --name ron-interview-aks --admin  
  ```
- perform upgrade to helm chart by increase revision number after making changes in chart  
- we can also change app version by edit `Chart.yaml`  
