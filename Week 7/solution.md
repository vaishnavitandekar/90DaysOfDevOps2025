# Week 7: Kubernetes Basics & Advanced Challenges – 90DaysOfDevOps

This document contains my solutions for **Week 7 Kubernetes tasks** using the **SpringBoot BankApp**.  
I have included YAML manifests, explanations, screenshots, and answers to interview-style questions.

---

## Task 1: Kubernetes Architecture & Deploy a Sample Pod

### 🏗 Kubernetes Architecture
- **Control Plane Components**
  - **API Server** – Entry point for all cluster communication.
  - **etcd** – Key-value store for cluster state.
  - **Scheduler** – Assigns pods to nodes.
  - **Controller Manager** – Ensures actual state = desired state.
  - **Cloud Controller** – Integrates with cloud providers.

- **Worker Node Components**
  - **Kubelet** – Communicates with API server, manages pods.
  - **Kube Proxy** – Handles networking & routing.
  - **Container Runtime** – Runs containers (Docker/Containerd).

### 📦 Pod YAML
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
  labels:
    app: nginx
spec:
  containers:
  - name: nginx
    image: nginx:latest
    ports:
    - containerPort: 80
````

Applied with:

```bash
kubectl apply -f pod.yaml
kubectl get pods
```

### Interview Q\&A

* **Q:** How do control plane components work together?

  **A:** API Server is the gateway, Scheduler assigns pods, Controller Manager maintains state, and etcd stores cluster data.

* **Q:** Pod fails to start – what do you check?

  **A:** Use `kubectl describe pod` and `kubectl logs <pod>` to check events, image issues, or resource limits.

---

## Task 2: Deployments, StatefulSets, DaemonSets & Namespaces

### Namespace YAML

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: bankapp-namespace
```

### Deployment YAML

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: bankapp-deployment
  namespace: bankapp-namespace
spec:
  replicas: 3
  selector:
    matchLabels:
      app: bankapp
  template:
    metadata:
      labels:
        app: bankapp
    spec:
      containers:
      - name: bankapp
        image: bankapp:latest
        ports:
        - containerPort: 8080
```

### StatefulSet YAML

```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: bankapp-db
  namespace: bankapp-namespace
spec:
  serviceName: "bankapp-db"
  replicas: 2
  selector:
    matchLabels:
      app: bankapp-db
  template:
    metadata:
      labels:
        app: bankapp-db
    spec:
      containers:
      - name: db
        image: mysql:8
        ports:
        - containerPort: 3306
        volumeMounts:
        - name: db-storage
          mountPath: /var/lib/mysql
  volumeClaimTemplates:
  - metadata:
      name: db-storage
    spec:
      accessModes: ["ReadWriteOnce"]
      resources:
        requests:
          storage: 1Gi
```

### DaemonSet YAML

```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: log-agent
  namespace: bankapp-namespace
spec:
  selector:
    matchLabels:
      name: log-agent
  template:
    metadata:
      labels:
        name: log-agent
    spec:
      containers:
      - name: log-agent
        image: fluentd:latest
```

### Interview Q\&A

* **Q:** How does a Deployment ensure desired state?

  **A:** It constantly compares actual pods with desired replicas; if pods die, new ones are created.

* **Q:** Deployment vs StatefulSet vs DaemonSet?

  **A:** Deployment = stateless apps, StatefulSet = stateful apps (DB), DaemonSet = one pod per node.

---

## Task 3: Networking & Exposure

### ClusterIP Service

```yaml
apiVersion: v1
kind: Service
metadata:
  name: bankapp-service
  namespace: bankapp-namespace
spec:
  selector:
    app: bankapp
  ports:
  - port: 80
    targetPort: 8080
  type: ClusterIP
```

### NodePort Service

```yaml
spec:
  type: NodePort
  ports:
  - port: 80
    targetPort: 8080
    nodePort: 30080
```

### Ingress Resource

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: bankapp-ingress
  namespace: bankapp-namespace
spec:
  rules:
  - host: bankapp.local
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: bankapp-service
            port:
              number: 80
```

### Network Policy

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: bankapp-deny-all
  namespace: bankapp-namespace
spec:
  podSelector: {}
  policyTypes:
  - Ingress
```

### Interview Q\&A

* **Q:** NodePort vs LoadBalancer?

  **A:** NodePort exposes service on each node, LoadBalancer provisions external LB.

* **Q:** Role of Network Policy?

  **A:** Restricts communication between pods, enforcing security.

---

## Task 4: Storage Management

### Persistent Volume (PV)

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-bankapp
spec:
  capacity:
    storage: 1Gi
  accessModes:
  - ReadWriteOnce
  hostPath:
    path: /mnt/data
```

### Persistent Volume Claim (PVC)

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pvc-bankapp
  namespace: bankapp-namespace
spec:
  accessModes:
  - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
```

### Pod Using PVC

```yaml
volumes:
- name: storage
  persistentVolumeClaim:
    claimName: pvc-bankapp
```

### Interview Q\&A

* **Q:** PV vs PVC?

  **A:** PV = actual storage, PVC = request for storage.

* **Q:** StorageClass?

  **A:** Automates dynamic storage provisioning in cloud.

---

## Task 5: ConfigMaps & Secrets

### ConfigMap

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: bankapp-config
  namespace: bankapp-namespace
data:
  DB_HOST: bankapp-db
  DB_PORT: "3306"
```

### Secret

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: bankapp-secret
  namespace: bankapp-namespace
type: Opaque
data:
  username: YWRtaW4=
  password: cGFzc3dvcmQ=
```

### Usage in Deployment

```yaml
envFrom:
- configMapRef:
    name: bankapp-config
- secretRef:
    name: bankapp-secret
```

### Interview Q\&A

* **Q:** Update ConfigMap/Secret?

  **A:** Modify YAML → `kubectl apply` → rolling restart for pods.

* **Q:** How to secure secrets?

  **A:** RBAC, encryption at rest, sealed-secrets, external vaults.

---

## Task 6: Autoscaling & Resource Management

### Resource Limits

```yaml
resources:
  requests:
    cpu: "100m"
    memory: "200Mi"
  limits:
    cpu: "500m"
    memory: "512Mi"
```

### HPA

```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: bankapp-hpa
  namespace: bankapp-namespace
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: bankapp-deployment
  minReplicas: 1
  maxReplicas: 5
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 50
```

### Interview Q\&A

* **Q:** How does HPA work?
  
  **A:** Uses Metrics Server → adjusts replicas based on CPU/memory usage.

* **Q:** When VPA?

  **A:** For apps that can’t scale horizontally (e.g., DB).

---

## Task 7: Security & Access Control

### Role

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: bankapp-namespace
  name: developer-role
rules:
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["get", "list", "create"]
```

### RoleBinding

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: developer-binding
  namespace: bankapp-namespace
subjects:
- kind: User
  name: developer
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role
  name: developer-role
  apiGroup: rbac.authorization.k8s.io
```

### Taints & Tolerations

```bash
kubectl taint nodes node1 key=value:NoSchedule
```

### Pod Disruption Budget (PDB)

```yaml
apiVersion: policy/v1
kind: PodDisruptionBudget
metadata:
  name: bankapp-pdb
  namespace: bankapp-namespace
spec:
  minAvailable: 2
  selector:
    matchLabels:
      app: bankapp
```

### Interview Q\&A

* **Q:** How does RBAC secure a cluster?
  
  **A:** Ensures users only access permitted resources.

* **Q:** Why PDB?

  **A:** Ensures availability during maintenance.

---

## Task 8: Jobs, CronJobs & CRDs

### Job

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: db-migration
  namespace: bankapp-namespace
spec:
  template:
    spec:
      containers:
      - name: migration
        image: alpine
        command: ["echo", "Database Migration Complete"]
      restartPolicy: Never
```

### CronJob

```yaml
apiVersion: batch/v1
kind: CronJob
metadata:
  name: db-backup
  namespace: bankapp-namespace
spec:
  schedule: "0 0 * * *"
  jobTemplate:
    spec:
      template:
        spec:
          containers:
          - name: backup
            image: alpine
            command: ["echo", "Database Backup Complete"]
          restartPolicy: OnFailure
```

### CRD (Custom Resource Definition)

```yaml
apiVersion: apiextensions.k8s.io/v1
kind: CustomResourceDefinition
metadata:
  name: bankpolicies.myorg.com
spec:
  group: myorg.com
  versions:
  - name: v1
    served: true
    storage: true
    schema:
      openAPIV3Schema:
        type: object
  scope: Namespaced
  names:
    plural: bankpolicies
    singular: bankpolicy
    kind: BankPolicy
```

### Interview Q\&A

* **Q:** Job vs CronJob?

  **A:** Job = one-time, CronJob = scheduled.

* **Q:** Why CRD?

  **A:** To extend Kubernetes with custom resources.

---

## Task 9: Bonus – Helm

### Helm Chart Deployment

```bash
helm create bankapp-chart
helm install bankapp ./bankapp-chart
helm upgrade bankapp ./bankapp-chart
```

### Interview Q\&A

* **Q:** How does Helm help?
  
  **A:** Package manager for Kubernetes – simplifies install & upgrades.

* **Q:** Why Service Mesh?

  **A:** Provides traffic management, security, observability for microservices.

---
