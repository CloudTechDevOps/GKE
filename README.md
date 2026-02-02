# Commands to Install GCP Tools and GKE

> Note: These commands are for Debian/Ubuntu-based systems and require sudo/root access.

#🧰 Step 1: Remove any broken repository entries
```
sudo rm -f /etc/apt/sources.list.d/kubernetes.list
sudo rm -f /etc/apt/sources.list.d/google-cloud-sdk.list
sudo rm -f /etc/apt/keyrings/kubernetes-apt-keyring.gpg
sudo rm -f /usr/share/keyrings/cloud.google.gpg
```
## 🔑 2. Re-add Google Cloud SDK repo (with correct key)
```
sudo apt update
sudo apt install -y apt-transport-https ca-certificates curl gnupg
```
# Add Google Cloud public key
```
curl -fsSL https://packages.cloud.google.com/apt/doc/apt-key.gpg | \
sudo gpg --dearmor -o /usr/share/keyrings/cloud.google.gpg

# Add the repo
echo "deb [signed-by=/usr/share/keyrings/cloud.google.gpg] \
https://packages.cloud.google.com/apt cloud-sdk main" | \
sudo tee /etc/apt/sources.list.d/google-cloud-sdk.list
```
##    ☸️ 3. Re-add Kubernetes apt repo
```
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.31/deb/Release.key | \
sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg

echo "deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] \
https://pkgs.k8s.io/core:/stable:/v1.31/deb/ /" | \
sudo tee /etc/apt/sources.list.d/kubernetes.list
```
##  🆙 4. Update repositories
```
sudo apt update
```
#🔧 5. Install the plugin

#Now install the GKE auth plugin:
```
sudo apt install -y google-cloud-sdk-gke-gcloud-auth-plugin
```
## Install kubectl
```
sudo apt-get install kubectl

sudo apt update && apt install -y vim

```
##  Then activate it:
```
export USE_GKE_GCLOUD_AUTH_PLUGIN=True
```

📌 1. Ensure Cloud SDK is Installed

Run this in your VM terminal:
```
gcloud --version
kubectl version
```
📌 2. Authenticate gcloud (if needed)  (optional)

If you didn't use a service account or need user login:
```
gcloud auth login		

####If you're using the VM with a default service account, you likely already have access. You can check active account:

gcloud config list account
```

📌 3. Set Your GCP Project and Zone/Region
```
gcloud config set project <YOUR_PROJECT_ID>  #(Replace YOUR_PROJECT_ID with your actual GCP project ID)


gcloud config set compute/zone us-central1-a   # Or any zone you prefer

```


#📌 4. Enable Kubernetes Engine API
```
gcloud services enable container.googleapis.com
```

 📌 5. Create a GKE Cluster

👉 Option A: Standard Cluster (manual node management)
```
gcloud container clusters create veera \
  --num-nodes=3 \
  --enable-ip-alias

```
📌 6. Get Cluster Credentials

Once the cluster is created, configure kubectl:

For Standard Cluster:
```
gcloud container clusters get-credentials veera \
  --location us-central1-a

```
```
kubectl get nodes
```
# run the ``` loadbalmncer-service.yaml``` placed on this repo just run and acces the application throgh loadbalncer ip
```
kubectl apply -f loadbalancer-service.yaml
```
To delete the cluster use below command
```
gcloud container clusters delete veera--zone us-central1-a
```

---









