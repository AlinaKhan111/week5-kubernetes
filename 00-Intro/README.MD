#  Kubernetes Basics & Terminology

##  What is Kubernetes?

**Kubernetes** (also known as **K8s**) is an **open-source container orchestration platform** used to deploy, manage, and scale containerized applications automatically.

It ensures applications run reliably across different environments — local, on-premise, or cloud — and is a core technology in **DevOps**.

---

##  Why Use Kubernetes?

-  Automates container deployment, scaling, and recovery
-  Ensures high availability and zero downtime
-  Uses declarative YAML for configuration
-  Makes scaling and rollback seamless

---

##  Core Concepts & Terminologies


### 1️. Cluster

A **Cluster** is a set of machines (VMs or physical servers) that run Kubernetes. It includes:

- **Control Plane (Master Node)** – Manages the cluster (scheduling, scaling, etc.)
- **Worker Nodes** – Where the actual containerized apps run

---

### 2️. Node

A **Node** is a single machine (VM or physical server) in the cluster.

- **Master Node** runs control plane components like:
  - API Server
  - Scheduler
  - Controller Manager
- **Worker Node** runs:
  - Pods (which contain containers)

---

### 3️. Pod

A **Pod** is the **smallest unit in Kubernetes**. It can run one or more containers that:

- Share the same IP address and port space
- Can share storage volumes
- Are always scheduled together on the same node

>  Usually one container per pod  
>  Multi-container pods are for tightly-coupled workloads

![Image](https://github.com/user-attachments/assets/3696090e-0a0b-4275-bbd3-959e30f83bec)



**Example:**  
A web app container and a log collector container sharing logs inside one pod.

---

### 4️. Containers in Pods

Kubernetes **manages Pods**, not containers directly.

- Containers are run using container runtimes like **Docker** or **containerd**
- Pods wrap containers and provide:
  - Shared networking (localhost communication)
  - Coordinated lifecycle
  - Health checks

---

### 5️. Deployment

A **Deployment** defines the desired state for a set of Pods and handles:

- Pod creation
- Replica count
- Rolling updates & rollbacks

**Sample Deployment YAML:**

```BASH
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        - name: nginx
          image: nginx
          ports:
            - containerPort: 80
```

### 6️. Service
A Service provides stable networking for Pods, enabling communication inside or outside the cluster.

Types of Services:
| Type         | Description                                      |
|--------------|--------------------------------------------------|
| ClusterIP    | Default. Internal-only access                    |
| NodePort     | Opens a port on every node for external access   |
| LoadBalancer | Assigns a public IP using cloud provider         |


### 7️. YAML Files
Kubernetes resources (like Pods, Deployments, Services) are defined using YAML files.

Example command:
```bash
kubectl apply -f deployment.yaml
```

This creates resources declaratively, as described in the YAML file.

### 8️. kubectl
kubectl is the command-line tool to interact with the Kubernetes cluster.

Common Commands:

```bash
kubectl get pods
kubectl create deployment nginx --image=nginx
kubectl expose deployment nginx --type=NodePort --port=80
kubectl delete pod <pod-name>
```

## Kubernetes Architecture
![Image](https://github.com/user-attachments/assets/505ce486-6189-4bb7-afba-ec076bb6c5e0)




## How Everything Connects
- Container runs inside a Pod

- Pods are grouped and managed by a Deployment

- Services expose Pods internally or externally

- kubectl and YAML files are used to manage everything declaratively

## Summary Table
| Term       | Description                                                  |
|------------|--------------------------------------------------------------|
| Cluster    | Group of machines running Kubernetes                         |
| Node       | Individual machine (VM or server) in the cluster             |
| Pod        | Smallest deployable unit (holds 1+ containers)               |
| Container  | Actual application process (Docker container)                |
| Deployment | Ensures Pods are running, manages updates/rollbacks          |
| Service    | Exposes Pods via ClusterIP, NodePort, or LoadBalancer (LB)   |
| kubectl    | CLI to manage Kubernetes resources                           |
| YAML       | Config format for declaring Kubernetes resources             |


## Conclusion:
Kubernetes allows you to deploy and manage scalable, resilient, and portable applications across cloud and on-premise environments using simple configuration and automation.
