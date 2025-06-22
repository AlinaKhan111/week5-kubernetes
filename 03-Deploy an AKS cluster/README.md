# Task 3: Deploy AKS Cluster using Azure Portal, Access Dashboard & Create Roles

##  Objective:
Deploy an AKS cluster using Azure Portal, access the Kubernetes Dashboard, and configure Role-Based Access Control (RBAC) for multiple users.

---

## Prerequisites:
- Azure Student Subscription (resource quota may be limited)
- Azure Portal Access: https://portal.azure.com
- Azure Cloud Shell enabled (Bash preferred)

---

##  Region Recommendation:
Use a region that supports AKS and VM sizes commonly allowed in student subscriptions.

**Recommended Region:** `East US`  
**Avoid:** `Central India` (no zone support, limited VM sizes)

---

##  VM Size Recommendation:
If `Standard_DS2_v2` is not available, choose:

-  `Standard_B2s` (2 vCPU, 4 GB RAM — budget-friendly)
-  `Standard_D2s_v3` (2 vCPU, 8 GB RAM — more powerful)

---

##  Step-by-Step: Deploy AKS via Azure Portal

1. **Go to Azure Portal**
   - https://portal.azure.com

2. **Search for "Kubernetes Services" → Click "Create"**

3. **Basics Tab**
   - Subscription: *Your Student Subscription*
   - Resource Group: `week5` or create new
   - Cluster name: `aks-cluster`
   - Region: `East US`
   - Kubernetes version: Leave default

4. **Node Pools Tab**
   - Node size: `Standard_B2s` or `Standard_D2s_v3`
   - Node count: `1`
   - Availability zones: **Uncheck all (set to None)**

5. **Authentication Tab**
   - Leave default (System-assigned managed identity)

6. **Networking Tab**
   - Network configuration: `Kubenet` (default)

7. **Review + Create**
   - Validate the config and click **"Create"**

---

##  Connect to AKS via Azure Cloud Shell

1. Open **Cloud Shell** (`>_` icon in top bar)
2. Run the following command to configure `kubectl`:

 ```bash
   az aks get-credentials --resource-group week5 --name aks-cluster
   ```

3. Verify connection:

```bash
kubectl get nodes
```

## Install Kubernetes Dashboard
```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.7.0/aio/deploy/recommended.yaml
```
Then run:

```bash
kubectl proxy
```

Open this link in your browser:
➡️ http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/


## Create Admin User for Dashboard Access
```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.7.0/docs/user/access-control/creating-sample-user.yaml
```
Generate login token:

```bash
kubectl -n kubernetes-dashboard create token admin-user
```

➡️ Use this token to log in to the Dashboard.

## Create Custom RBAC Roles for Other Users
Example: Create a readonly-user with access to view Pods & Services.

1. Create a file called readonly-user.yaml:

```bash
apiVersion: v1
kind: ServiceAccount
metadata:
  name: readonly-user
  namespace: default
---
kind: Role
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  namespace: default
  name: readonly-role
rules:
- apiGroups: [""]
  resources: ["pods", "services"]
  verbs: ["get", "list", "watch"]
---
kind: RoleBinding
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: readonly-rolebinding
  namespace: default
subjects:
- kind: ServiceAccount
  name: readonly-user
  namespace: default
roleRef:
  kind: Role
  name: readonly-role
  apiGroup: rbac.authorization.k8s.io
```

2. Apply the file:
```bash
kubectl apply -f readonly-user.yaml
```
3. Generate token for the new user:
```bash
kubectl -n default create token readonly-user
```

➡️ Use this token to log in to the dashboard with restricted access.


### Notes:
- Always check region + VM size availability if issues occur.

- For more roles: Use ClusterRole and ClusterRoleBinding for access across namespaces.

- Use kubectl delete to clean up any RBAC objects or deployments after testing.