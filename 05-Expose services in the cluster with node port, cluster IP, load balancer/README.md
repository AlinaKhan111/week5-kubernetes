#  Task 5: Expose Services in AKS using ClusterIP, NodePort, and LoadBalancer

##  Objective:
Deploy an application to AKS and expose it in three different ways:
1. `ClusterIP` – internal communication within the cluster
2. `NodePort` – access using VM's public IP and high port
3. `LoadBalancer` – access over the internet via a public IP

---

##  Application Used:
For demonstration, we’ll use the official `nginx` image.

---

##  Step-by-Step Guide;

### 1️. Connect to AKS

```bash
az aks get-credentials --resource-group week5 --name aks-cluster
kubectl get nodes
```

### 2️. Create Deployment
```bash
kubectl create deployment nginx-app --image=nginx
kubectl get pods
```
## A. Expose via ClusterIP (Default – internal only)
```bash
kubectl expose deployment nginx-app --name=nginx-clusterip --port=80 --target-port=80 --type=ClusterIP
```

Check service:
```bash
kubectl get service nginx-clusterip
```
 This service is only accessible inside the cluster.

You can test from a pod:
```bash
kubectl run test-pod --rm -i -t --image=busybox -- /bin/sh
wget -qO- http://nginx-clusterip
```

## B. Expose via NodePort (Access via Node Public IP + Port)
```bash
kubectl expose deployment nginx-app --name=nginx-nodeport --port=80 --target-port=80 --type=NodePort
```

Get service:
```bash
kubectl get service nginx-nodeport
```

Example output:

```bash
NAME              TYPE       CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE
nginx-nodeport    NodePort   10.0.115.244   <none>        80:31234/TCP   1m
```
➡️ Access using:
```bash
http://<AKS-Node-IP>:<NodePort>
```

To get Node IP:
```bash
kubectl get nodes -o wide
```

## C. Expose via LoadBalancer (Access over Public Internet)
```bash
kubectl expose deployment nginx-app --name=nginx-loadbalancer --port=80 --target-port=80 --type=LoadBalancer
```

Get service:
```bash
kubectl get service nginx-loadbalancer
```

Wait 1–2 minutes for EXTERNAL-IP to appear.

➡️ Access:
```bash
http://<EXTERNAL-IP>
```

## Summary:
| Service Type | Command Used                          |
|--------------|----------------------------------------|
| ClusterIP    | `kubectl expose ... --type=ClusterIP`  |
| NodePort     | `kubectl expose ... --type=NodePort`   |
| LoadBalancer | `kubectl expose ... --type=LoadBalancer`|

### ✅ My Execution Notes:

- ClusterIP: Verified by running a BusyBox test pod and using `wget` on the internal ClusterIP. Response from NGINX confirmed successful internal access.
- NodePort: Service exposed on port 31591. Since AKS nodes do not have public IPs, this method could not be tested externally — which is expected behavior in AKS.
- LoadBalancer: Created successfully, but EXTERNAL-IP remained `<pending>` due to Azure Student Subscription limitations in  region.


![Image](https://github.com/user-attachments/assets/1c0a2246-cab2-4046-a275-4e8525769085)
![Image](https://github.com/user-attachments/assets/65fa34a0-93a9-49cd-8a30-043ec9f5dc8f)
![Image](https://github.com/user-attachments/assets/516240aa-d053-41df-adcb-2bfcfb5cc877)

