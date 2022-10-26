# Hitesh_devops_assesment
## Prerequisites Installation and verification.
Make Installation:
```
sudo apt-get install make
```
 
Docker Installation:
```
sudo apt-get update
sudo apt-get install \
   ca-certificates \
   curl \
   gnupg \
   lsb-release

sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
echo \
"deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
$(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
```

## environment preparation
KD3 Installation.
```
wget -q -O - https://raw.githubusercontent.com/k3d-io/k3d/main/install.sh | bash
```
kubectl Installation.
```
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
chmod +x kubectl
sudo mv kubectl /usr/local/bin/kubectl
```
Helm Installation
```
wget https://get.helm.sh/helm-v3.10.1-linux-amd64.tar.gz
unzip file
and move it  /usr/local/bin/helm
```

## cluster preparation
changed arg in Make file  --k3s-server-arg '--no-deploy=traefik' \   to  --k3s-arg '--no-deploy=traefik' \
 ```make cluster```

if your machine rebooted then need to start cluster
```
sudo k3d cluster start sandman
sudo kubectl create  
```

## Nginx Ingress Controller
```
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm install ingress-nginx ingress-nginx/ingress-nginx -n ingress-nginx --create-namespace --set controller.publishService.enabled=true
```

## Prometheus Installation & Configuration
```
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm install prometheus prometheus-community/prometheus -n prometheus --create-namespace
kubectl apply -f ingress-k8s-manifest/prometheus.yaml
```

## Application Deployment
```
kubectl create app
cd sample-service/k8s-manifest/
kubectl apply -f .
```

## Jenkins Deployment
```
helm repo add bitnami https://charts.bitnami.com/bitnami
kubectl create ns jenkins
helm install jenknis bitnami/jenkins
kubectl apply -f ingress-k8s-manifest/jenkins.yaml
```

## Application CICD Configuration.
* Create a pipeline job into jenkins called sample-service
* Go to SCM section and select GIT and enter your github repo URL
* Jenkinsfile path should ```sample-service```
* Add docker hub creds into globle creds into jenkins
   * create docker repo into your docker account
* Run the job if all parameters are correct application build and deploy into your k3d cluster