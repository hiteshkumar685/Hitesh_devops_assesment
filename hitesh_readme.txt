
**environment preparation**
1 Installed KD3 using wget -q -O - https://raw.githubusercontent.com/k3d-io/k3d/main/install.sh | bash

2 installed kubectl 
   curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
   chmod +x ./kubectl
   sudo mv ./kubectl /usr/local/bin/kubectl
3  Installed Helm
   wget https://get.helm.sh/helm-v3.10.1-linux-amd64.tar.gz
   unzip file
  and move it  /usr/local/bin/helm
cluster preparation
4 changed arg in make file  --k3s-server-arg '--no-deploy=traefik' \   to  --k3s-arg '--no-deploy=traefik' \
 run command   make cluster


if your machine rebooted then need to start cluster 
sudo k3d  cluster start sandman
sudo kubectl create  

sudo helm repo add nginx-stable https://helm.nginx.com/stable
sudo helm repo update
sudo helm install nginx-ingress nginx-stable/nginx-ingress --set rbac.create=true
sudo kubectl create -f prometheus/namespace.yml
sudo kubectl apply --kustomize prometheus/

sudo k3d  cluster start sandman





https://octopus.com/blog/jenkins-helm-install-guide
Admin password for jenkins:  J7hbuGnusBeIlUjlbaM550
instaled jenkins plugin for docker Docker API Plugin
Docker Commons Plugin
Docker Pipeline
Docker Plugin
docker-build-step
sudo kubectl --namespace default port-forward svc/myjenkins 8081:8080 
