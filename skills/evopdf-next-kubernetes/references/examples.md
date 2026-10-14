# Kubernetes: samples

Every code sample of the documentation topics behind this skill, complete and in the order of the topic.

## Using EvoPdf Next for .NET in Kubernetes

Topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/publish-to-kubernetes.htm

### Create the AKS Cluster

```bash
az aks create --resource-group pdf-app-rg --name pdf-aks --node-count 1 --node-vm-size Standard_D2s_v3 \
  --attach-acr myregistry --generate-ssh-keys
az aks get-credentials --resource-group pdf-app-rg --name pdf-aks
kubectl get nodes
```

### Deploy the Application

**pdf-app.yaml**

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: pdf-app
spec:
  replicas: 1
  selector:
    matchLabels:
      app: pdf-app
  template:
    metadata:
      labels:
        app: pdf-app
    spec:
      containers:
      - name: pdf-app
        image: myregistry.azurecr.io/pdf-app:1
        ports:
        - containerPort: 8080
        resources:
          requests:
            cpu: 500m
            memory: 1Gi
          limits:
            memory: 4Gi
---
apiVersion: v1
kind: Service
metadata:
  name: pdf-app
spec:
  type: LoadBalancer
  selector:
    app: pdf-app
  ports:
  - port: 80
    targetPort: 8080
```

```bash
kubectl apply -f pdf-app.yaml
kubectl rollout status deployment/pdf-app
kubectl get service pdf-app
```

### Amazon EKS

```bash
mkdir -p ~/bin
curl -sL "https://github.com/eksctl-io/eksctl/releases/latest/download/eksctl_Linux_amd64.tar.gz" | tar -xz -C ~/bin
curl -sLO "https://dl.k8s.io/release/$(curl -Ls https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl" && chmod +x kubectl && mv kubectl ~/bin/
eksctl create cluster --name pdf-eks --region us-east-1 --nodegroup-name ng --node-type m5.xlarge --nodes 1 --managed
```

```bash
kubectl delete -f pdf-app.yaml
eksctl delete cluster --name pdf-eks --region us-east-1
```

### Google Kubernetes Engine

```bash
gcloud services enable container.googleapis.com
gcloud container clusters create pdf-gke --zone us-central1-a --num-nodes 1 --machine-type n2-standard-4 --disk-size 50
gcloud container clusters get-credentials pdf-gke --zone us-central1-a
PROJECT_NUMBER=$(gcloud projects describe PROJECT_ID --format="value(projectNumber)")
gcloud projects add-iam-policy-binding PROJECT_ID --member="serviceAccount:$PROJECT_NUMBER-compute@developer.gserviceaccount.com" --role="roles/artifactregistry.reader"
```

### Troubleshooting

```bash
kubectl get pods -l app=pdf-app
kubectl describe pod -l app=pdf-app
kubectl logs -l app=pdf-app --tail 100
```
