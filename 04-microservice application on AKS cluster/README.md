#  Task 4: Deploy a Microservice Application on AKS and Access It via Public Internet

##  Objective:
Deploy a sample microservice-based application (e.g., Nginx or custom Flask app) into the AKS cluster created in Task 3, and expose it using a public-facing service (LoadBalancer).

---

##  Application Used:
We will use a basic **Nginx deployment** to simulate a microservice. You can replace this with your own app (e.g., Flask, Node.js, etc.) later.

---

##  Step-by-Step Guide

### 1. Open Azure Cloud Shell
Launch the **Azure Cloud Shell** from the top bar in Azure Portal (Bash preferred).

---

### 2️. Connect to Your AKS Cluster

```bash
az aks get-credentials --resource-group week5 --name aks-cluster
```

Verify connection:
```bash
kubectl get nodes
```

### 3. Create a Deployment for the Microservice
```bash
kubectl create deployment webapp --image=nginx
```

Check deployment:
```bash
kubectl get deployments
kubectl get pods
```

### 4️. Expose the Application to Public Internet
```bash
kubectl expose deployment webapp --port=80 --type=LoadBalancer
```
This creates a Service of type LoadBalancer, which provisions a public IP.

### 5. Get the Public IP
```bash
kubectl get service webapp
```
Look for the `EXTERNAL-IP` field. It might take 1–2 minutes to appear.

Example Output:

```bash
NAME     TYPE           CLUSTER-IP     EXTERNAL-IP     PORT(S)        AGE
webapp   LoadBalancer   10.0.123.45    52.168.21.15    80:30899/TCP   2m
```

➡️ Access your app in a browser:
http://<EXTERNAL-IP>
Example: http://52.168.21.15


## Summary:

- Connected to AKS cluster
- Microservice (Nginx) deployed	
- Service of type LoadBalancer created
- Application accessible via public IP


## Notes:
- If EXTERNAL-IP stays <pending>, wait a few minutes or ensure your cluster is in a region that supports public LoadBalancers (e.g., East US).

- Replace Nginx with your custom container image by updating the deployment:

```bash
kubectl set image deployment/webapp webapp=<your-image>
```
- To delete everything later:
```bash
kubectl delete service webapp
kubectl delete deployment webapp
```
![Image](https://github.com/user-attachments/assets/bb2c4dcd-4d81-4e61-bcd3-2b8fed7b1ae8)
![Image](https://github.com/user-attachments/assets/333de708-e0b3-48d0-a73e-d48730bb20b6)
