# Task 3: Deploy AKS Cluster using Azure Portal, Access Dashboard & Create Roles

##  Objective:
Deploy an AKS cluster using Azure Portal, access the Kubernetes Dashboard, and configure Role-Based Access Control (RBAC) for multiple users.

---

##  Prerequisites:
- Azure Student Subscription (may have quota/VM limits)
- Azure Portal: [https://portal.azure.com](https://portal.azure.com)
- Azure Cloud Shell (Bash preferred)

---

##  Region & VM Selection:
Due to regional and VM size constraints in Azure Student Subscription:

- **Selected Region:** Southeast Asia  
- **Selected VM Size:** Standard_A2_v2 (only allowed size in this region)

---

##  AKS Deployment via Azure Portal

1. **Navigate** to Azure Portal → Search "Kubernetes Services" → Click **Create**
2. **Basics Tab:**
   - Subscription: Student Subscription
   - Resource Group: `week5`
   - Cluster Name: `aks-cluster111`
   - Region: `Southeast Asia`
3. **Node Pools Tab:**
   - Node Size: `Standard_A2_v2`
   - Node Count: 1
   - Availability Zones: Unchecked (None)
4. **Authentication:** System-assigned managed identity (default)
5. **Networking:** Kubenet (default)
6. **Review + Create:** ✅ Deployment successful

---

##  Connected via Azure Cloud Shell

```bash
az aks get-credentials --resource-group week5 --name aks-cluster111
kubectl get nodes
```
 Verified 2 nodes in Ready state.

## Installed Kubernetes Dashboard
```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.7.0/aio/deploy/recommended.yaml
kubectl get pods -n kubernetes-dashboard
```
 Dashboard pod and metrics scraper were in Running state.

## Created Admin User & Generated Token
```bash

# Applied admin user manifest
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.7.0/aio/deploy/recommended.yaml

# Verified ServiceAccount
kubectl -n kubernetes-dashboard get serviceaccount admin-user -o yaml

# Created ClusterRoleBinding
kubectl get clusterrolebinding admin-user -o yaml

# Generated login token
kubectl -n kubernetes-dashboard create token admin-user
```
Token successfully generated.

## Attempted Dashboard Access
```bash

kubectl proxy
```
## Output:

```bash
Starting to serve on 127.0.0.1:8001
```

### ⚠️ Access Limitation Justification
Despite following all correct steps:

Dashboard access via browser (http://localhost:8001/...) failed because:

- Cloud Shell cannot preview local ports like localhost:8001.

- External IP access for dashboard service also didn’t work due to security restrictions in AKS/Cloud Shell.

### Successful Configuration
- AKS cluster deployed in constrained environment (Southeast Asia, A2_v2)

- Dashboard installed and verified via kubectl

- Admin user created and bound with cluster-admin privileges

- Login token generated

- Dashboard proxy was successfully started via kubectl proxy

📸 Screenshots Taken


![Image](https://github.com/user-attachments/assets/be9f4295-d739-4cc9-b63c-f81c1a6f0dfe)
![Image](https://github.com/user-attachments/assets/4006dc2a-48a0-475e-903b-7a3eddd47f2f)
![Image](https://github.com/user-attachments/assets/9d853c8f-6da0-46e3-b96c-dda3ea2b1cc5)
![Image](https://github.com/user-attachments/assets/5b103354-c80b-4a4d-b2be-a66497d70b71)




