# Dados no Kubernetes [DoK]
### stack-dados-k8s

# https://medium.com/@ahmetfurkandemir/kubernetes-with-data-engineering-approach-1-create-a-kubernetes-cluster-with-all-its-0a71ae686d97#id_token=eyJhbGciOiJSUzI1NiIsImtpZCI6ImUxYjkzYzY0MDE0NGI4NGJkMDViZjI5NmQ2NzI2MmI2YmM2MWE0ODciLCJ0eXAiOiJKV1QifQ.eyJpc3MiOiJodHRwczovL2FjY291bnRzLmdvb2dsZS5jb20iLCJhenAiOiIyMTYyOTYwMzU4MzQtazFrNnFlMDYwczJ0cDJhMmphbTRsamRjbXMwMHN0dGcuYXBwcy5nb29nbGV1c2VyY29udGVudC5jb20iLCJhdWQiOiIyMTYyOTYwMzU4MzQtazFrNnFlMDYwczJ0cDJhMmphbTRsamRjbXMwMHN0dGcuYXBwcy5nb29nbGV1c2VyY29udGVudC5jb20iLCJzdWIiOiIxMDY1NTQ0NDYxMDI2Mjk2MDMyMzkiLCJlbWFpbCI6InJvZHJpZ28ucm9tYW56aW5pQGdtYWlsLmNvbSIsImVtYWlsX3ZlcmlmaWVkIjp0cnVlLCJuYmYiOjE3MTQ0OTgxNjEsIm5hbWUiOiJSb2RyaWdvIFJvbWFuemluaSIsInBpY3R1cmUiOiJodHRwczovL2xoMy5nb29nbGV1c2VyY29udGVudC5jb20vYS9BQ2c4b2NKQ2ptQnFyM3pIM3ZWRXVicHNnLXo2MWV6a0s0N3dZZ1BlRVF0X0lfZl9OMXprQ3c9czk2LWMiLCJnaXZlbl9uYW1lIjoiUm9kcmlnbyIsImZhbWlseV9uYW1lIjoiUm9tYW56aW5pIiwiaWF0IjoxNzE0NDk4NDYxLCJleHAiOjE3MTQ1MDIwNjEsImp0aSI6ImFhMjlmZDc0ZjZmZGQ2ZmE4NTNlYjE5ZTExZTdjNWI5Y2IwMTZiMWUifQ.q9nH_8bRbv1lfOTvwthYTusoKWxADoNHPYC5eczneOo1pJ0TeQhFo2eFB3YovEfBvTcy_34UGE48EQoNyDKzFDr5L9Nw4BG__CPEqpQyNPt9RZLJgOo55X-YX8mx530t1XrEz3R7CgNFWM1v5V0cImNuvKJfvJJtE4qCLGBMDnWW2zsQr3B-XakboKlv_ByQ8jiWQEt47s46oWzQEwzKtrPQr620PwM26uSsb5iPdY0tzwuyE2TIylO3gf_iRUZ7rV7y4pl931L6pfzvakN7dX7U3zPWvDUYY4IqXsbP98E7DqJUxM4cdecs_byVJr9rt4UF3MZH3BHVYn81vNKvmQ

# https://medium.com/@ahmetfurkandemir/kubernetes-with-data-engineering-approach-2-querying-to-object-storage-minio-with-trino-using-ae68ffbde9f9

# https://minikube.sigs.k8s.io/docs/start/

# 

# Inicializando o GIT
git config --global user.name "Rodrigo Romanzini"
git config --global user.email "rodrigo.romanzini@gmail.com"
git init
git add .
git commit -m "teste novo commig"
git branch -M main
git remote add origin https://git@github.com/romanzini/stack-dados-k8s.git
git push -u origin main


Criando um ambiente de dados no kubernetes utilizando os mais famosos produtos open-source do mercado de dados.

Esse projeto está estruturado em 3 partes:
- [infra](#infra)
- [apps](#apps)
- [data](#data-cluster)

### infra
Todos os recursos necessários para criar um cluster de kubernetes assim como os compomentes necessários
para o ambiente de dados utilizando GitOps.

Para deployment do ambiente, siga os passos:
0. WSL2
1. Docker
2. kubectl
3. Minikube
4. Helm
5. Kubernetes

# Instalando o WSL
0. wsl --list --online
1. wsl --install -d Ubuntu-22.04

# Atualizando o sistema
0. sudo apt update
1. sudo apt upgtade

# Instalando o kubectl
0. Download the latest release with the command
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
1. Validate the binary (optional)
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl.sha256"
2. Install kubectl
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
3. Test to ensure the version you installed is up-to-date:
kubectl version --client

# Instalando o minikube
0. To install the latest minikube stable release on x86-64 Linux using binary download
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube && rm minikube-linux-amd64
1. Initialize cluster
minikube start
2. Interact with cluster
kubectl get po -A
3. Initialize Kubernetes Dashboard
minikube dashboard

# Installing minikube dashboard
0. Installing Kubernetes Metric API (file with code in kubernetes-metric-api.yaml)
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/high-availability.yaml
kubectl apply -f infra/kubernetes/kubernetes-metric-api.yaml

# Kubernetes Storage configuration (optional)
0. Create file with this name StorageClass.yaml and put de code inside
kubectl apply -f infra/kubernetes/StorageClass.yaml

# Installing Helm
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
chmod 700 get_helm.sh
./get_helm.sh

# Creating a Virtual Environment
 sudo apt install python3.10-venv
 python3 -m venv stack-dados-k8s
 source stack-dados-k8s/bin/activate
 deactivate  stack-dados-k8s

 # Installing ArgoCD on Kubernetes
 helm repo add argo https://argoproj.github.io/argo-helm
 kubectl create namespace gitops
 kubectl apply -n gitops -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
 kubectl patch svc argocd-server -n gitops -p '{"spec": {"type": "LoadBalancer"}}'  ###### (não executar)######
 kubectl port-forward svc/argocd-server -n gitops 8080:443
  
 kubectl get svc argocd-server -n gitops
 kubectl -n gitops get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo
 (lCgt0Vt1MrrqnARQ)

 kubectl apply -f infra/gitops/git-repo-con.yaml -n gitops