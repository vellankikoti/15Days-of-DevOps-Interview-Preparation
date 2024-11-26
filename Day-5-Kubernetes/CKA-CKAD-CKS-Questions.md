### Certified Kubernetes Administrator (CKA) Sample Questions and Answers

---

### **Cluster Management**

**1. Task:**  
Create a ClusterRole named `deployment-clusterrole` allowing only the creation of:  
- Deployment  
- StatefulSet  
- DaemonSet  

Bind this role to a ServiceAccount named `cicd-token` in the `app-team1` namespace.

**Answer:**  
```yaml
# ClusterRole
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: deployment-clusterrole
rules:
- apiGroups: ["apps"]
  resources: ["deployments", "statefulsets", "daemonsets"]
  verbs: ["create"]

# ServiceAccount
apiVersion: v1
kind: ServiceAccount
metadata:
  name: cicd-token
  namespace: app-team1

# RoleBinding
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: bind-cicd-role
  namespace: app-team1
subjects:
- kind: ServiceAccount
  name: cicd-token
  namespace: app-team1
roleRef:
  kind: ClusterRole
  name: deployment-clusterrole
  apiGroup: rbac.authorization.k8s.io
```

---

### **Node Management**

**2. Task:**  
Set the node `ek8s-node-0` as unavailable and reschedule all Pods running on it.

**Answer:**  
```bash
kubectl cordon ek8s-node-0
kubectl drain ek8s-node-0 --ignore-daemonsets --delete-emptydir-data
```

---

**3. Task:**  
Upgrade the Kubernetes control plane on the master node from version `1.22.1` to `1.22.2`.

**Answer:**  
```bash
# First, drain the node
kubectl drain <master-node> --ignore-daemonsets --delete-emptydir-data

# Upgrade kubeadm
sudo apt-get install -y kubeadm=1.22.2-00

# Plan and apply the upgrade
sudo kubeadm upgrade plan
sudo kubeadm upgrade apply v1.22.2

# Upgrade kubelet and kubectl
sudo apt-get install -y kubelet=1.22.2-00 kubectl=1.22.2-00

# Restart kubelet
sudo systemctl restart kubelet

# Uncordon the master node
kubectl uncordon <master-node>
```

---

### **Etcd Backup and Restore**

**4. Task:**  
Create a snapshot of the etcd database and restore a previous snapshot.

**Answer:**  
```bash
# Backup current etcd instance
ETCDCTL_API=3 etcdctl snapshot save /var/lib/backup/etcd-snapshot.db \
  --endpoints=https://127.0.0.1:2379 --cacert=/etc/kubernetes/pki/etcd/ca.crt \
  --cert=/etc/kubernetes/pki/etcd/server.crt --key=/etc/kubernetes/pki/etcd/server.key

# Restore from a previous snapshot
ETCDCTL_API=3 etcdctl snapshot restore /var/lib/backup/etcd-snapshot-previous.db \
  --data-dir=/var/lib/etcd-restored
```

---

### **Network Policies**

**5. Task:**  
Create a NetworkPolicy `allow-port-from-namespace` in the `fubar` namespace:  
- Allow traffic from Pods in `internal` namespace to Pods on port `9000` in `fubar`.  
- Ensure only Pods listening on port `9000` are accessible.  

**Answer:**  
```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-port-from-namespace
  namespace: fubar
spec:
  podSelector:
    matchLabels: {}
  ingress:
  - from:
    - namespaceSelector:
        matchLabels:
          name: internal
    ports:
    - protocol: TCP
      port: 9000
  policyTypes:
  - Ingress
```

---

### **Persistent Volumes**

**6. Task:**  
Create a PersistentVolume named `app-data`:  
- Capacity: 2Gi  
- Access Mode: ReadOnlyMany  
- Host Path: `/srv/app-data`  

**Answer:**  
```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: app-data
spec:
  capacity:
    storage: 2Gi
  accessModes:
  - ReadOnlyMany
  hostPath:
    path: /srv/app-data
```

---

**7. Task:**  
Create a PersistentVolumeClaim `pv-volume` with `ReadWriteOnce` access and expand it later to 70Mi.

**Answer:**  
```yaml
# PVC
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pv-volume
spec:
  accessModes:
  - ReadWriteOnce
  resources:
    requests:
      storage: 10Mi
---
# Expand the PVC
kubectl patch pvc pv-volume --patch '{"spec": {"resources": {"requests": {"storage": "70Mi"}}}}'
```

---

### **Logging**

**8. Task:**  
Monitor logs of a Pod and write error logs to a specific file.

**Answer:**  
```bash
kubectl logs <pod-name> | grep "error file-not-found" > /opt/KUTR00101/foo
```

---

**9. Task:**  
Integrate a streaming sidecar container for logging.

**Answer:**  
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: big-corp-app
spec:
  containers:
  - name: app-container
    image: busybox
    volumeMounts:
    - name: log-volume
      mountPath: /var/log
  - name: sidecar
    image: busybox
    command: ["/bin/sh", "-c", "tail -f /var/log/big-corp-app.log"]
    volumeMounts:
    - name: log-volume
      mountPath: /var/log
  volumes:
  - name: log-volume
    emptyDir: {}
```

---

**10. Task:**  
Find Pods consuming the most CPU and write the Pod name to a file.

**Answer:**  
```bash
kubectl top pod --sort-by=cpu | head -n 2 > /opt/KUTR00401/KUTR00401.txt
```

---

### **Workload Management**

**11. Task:**  
Scale the deployment `presentation` to 3 Pods.  

**Answer:**  
```bash
kubectl scale deployment presentation --replicas=3
```

---

**12. Task:**  
Schedule a Pod named `nginx-kusc00401` on a node with a label `disk=ssd`.  

**Answer:**  
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-kusc00401
spec:
  nodeSelector:
    disk: ssd
  containers:
  - name: nginx
    image: nginx
```

---

**13. Task:**  
Create a Pod named `kucc8` with two containers:  
- `nginx` container  
- `consul` container  

**Answer:**  
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: kucc8
spec:
  containers:
  - name: nginx
    image: nginx
  - name: consul
    image: consul
```

---

### **Storage Management**

**14. Task:**  
Create a PersistentVolumeClaim (PVC) named `nginx-pvc` with:  
- Storage Class: `fast-storage`  
- Capacity: 10Gi  
- Access Mode: `ReadWriteOnce`.  

**Answer:**  
```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: nginx-pvc
spec:
  storageClassName: fast-storage
  accessModes:
  - ReadWriteOnce
  resources:
    requests:
      storage: 10Gi
```

---

**15. Task:**  
Create a PersistentVolume and mount it to a Pod for `/usr/share/nginx/html`.  

**Answer:**  
```yaml
# PersistentVolume
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-example
spec:
  capacity:
    storage: 1Gi
  accessModes:
  - ReadWriteOnce
  hostPath:
    path: /data

# PVC
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pvc-example
spec:
  accessModes:
  - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi

# Pod
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
spec:
  containers:
  - name: nginx
    image: nginx
    volumeMounts:
    - name: example-volume
      mountPath: /usr/share/nginx/html
  volumes:
  - name: example-volume
    persistentVolumeClaim:
      claimName: pvc-example
```

---

### **Cluster Networking**

**16. Task:**  
Create an Ingress resource named `pong` in the namespace `ing-internal`, exposing:  
- Path `/hello`  
- Service `hello` on port `5678`.  

**Answer:**  
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: pong
  namespace: ing-internal
spec:
  rules:
  - http:
      paths:
      - path: /hello
        pathType: Prefix
        backend:
          service:
            name: hello
            port:
              number: 5678
```

---

**17. Task:**  
Create a NetworkPolicy allowing Pods in namespace `internal` to connect on port `8080` to Pods in namespace `web`.  

**Answer:**  
```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-namespace-access
  namespace: web
spec:
  podSelector:
    matchLabels: {}
  ingress:
  - from:
    - namespaceSelector:
        matchLabels:
          name: internal
    ports:
    - protocol: TCP
      port: 8080
```

---

**18. Task:**  
Expose an existing deployment `frontend` as a ClusterIP service on port `80`.  

**Answer:**  
```bash
kubectl expose deployment frontend --type=ClusterIP --port=80
```

---

**19. Task:**  
Find the number of `Ready` nodes (excluding nodes tainted `NoSchedule`) and save it to a file.  

**Answer:**  
```bash
kubectl get nodes --no-headers | grep -v "NoSchedule" | grep -c "Ready" > /opt/KUSC00402/kusc00402.txt
```

---

### **Control Plane Management**

**20. Task:**  
Troubleshoot a `NotReady` worker node and bring it to a `Ready` state.  

**Answer:**  
1. **Check Node Status:**  
   ```bash
   kubectl describe node <node-name>
   ```
2. **Common Checks:**  
   - Verify Kubelet is running:  
     ```bash
     systemctl status kubelet
     ```  
   - Check disk space:  
     ```bash
     df -h
     ```  
   - Restart kubelet if needed:  
     ```bash
     systemctl restart kubelet
     ```  
3. **Confirm Node is Ready:**  
   ```bash
   kubectl get nodes
   ```

---

**21. Task:**  
Create a Role allowing Pods to list all ConfigMaps in the `default` namespace, and bind it to a ServiceAccount named `reader`.  

**Answer:**  
```yaml
# Role
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: default
  name: configmap-reader
rules:
- apiGroups: [""]
  resources: ["configmaps"]
  verbs: ["list"]

# ServiceAccount
apiVersion: v1
kind: ServiceAccount
metadata:
  name: reader

# RoleBinding
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: bind-reader-role
  namespace: default
subjects:
- kind: ServiceAccount
  name: reader
roleRef:
  kind: Role
  name: configmap-reader
  apiGroup: rbac.authorization.k8s.io
```

---

**22. Task:**  
Restart all Pods in a specific namespace called `staging`.  

**Answer:**  
```bash
kubectl delete pod --all -n staging
```

---
### **Certified Kubernetes Application Developer (CKAD)** Sample Questions  

---

### **Workload Design**

**1. Task:**  
Create a Deployment named `nginx-deploy` with the following specifications:  
- Replica count: 3  
- Container image: `nginx:1.17`  
- Expose port `80`  

**Answer:**  
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deploy
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
        image: nginx:1.17
        ports:
        - containerPort: 80
```

---

**2. Task:**  
Scale down the above Deployment `nginx-deploy` to a single replica.  

**Answer:**  
```bash
kubectl scale deployment nginx-deploy --replicas=1
```

---

**3. Task:**  
Define a CronJob named `log-cleaner` that runs every day at midnight to execute the `echo "Logs Cleaned"` command.  

**Answer:**  
```yaml
apiVersion: batch/v1
kind: CronJob
metadata:
  name: log-cleaner
spec:
  schedule: "0 0 * * *"
  jobTemplate:
    spec:
      template:
        spec:
          containers:
          - name: cleaner
            image: busybox
            command: ["sh", "-c", "echo 'Logs Cleaned'"]
          restartPolicy: OnFailure
```

---

**4. Task:**  
Update the Deployment `nginx-deploy` to use the `nginx:1.18` image instead of `nginx:1.17`.  

**Answer:**  
```bash
kubectl set image deployment/nginx-deploy nginx=nginx:1.18
```

---

### **Service Design**

**5. Task:**  
Expose the Deployment `nginx-deploy` as a NodePort Service on port `30080`.  

**Answer:**  
```bash
kubectl expose deployment nginx-deploy --type=NodePort --port=80 --target-port=80 --node-port=30080
```

---

**6. Task:**  
Define a Headless Service for a StatefulSet with the name `web` to discover Pods directly.  

**Answer:**  
```yaml
apiVersion: v1
kind: Service
metadata:
  name: web
spec:
  clusterIP: None
  selector:
    app: web
  ports:
  - port: 80
    targetPort: 80
```

---

### **Pod Design**

**7. Task:**  
Create a Pod named `multi-container` with two containers:  
- `nginx` container using `nginx` image.  
- `redis` container using `redis` image.  

**Answer:**  
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: multi-container
spec:
  containers:
  - name: nginx
    image: nginx
  - name: redis
    image: redis
```

---

**8. Task:**  
Create a ConfigMap named `app-config` with the following key-value pairs:  
- `app_name=MyApp`  
- `app_env=production`  

Mount this ConfigMap into a Pod as environment variables.  

**Answer:**  
```yaml
# ConfigMap
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  app_name: MyApp
  app_env: production

# Pod
apiVersion: v1
kind: Pod
metadata:
  name: app-with-config
spec:
  containers:
  - name: app-container
    image: nginx
    envFrom:
    - configMapRef:
        name: app-config
```

---

**9. Task:**  
Create a Pod named `volume-pod` with an emptyDir volume mounted at `/data`.  

**Answer:**  
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: volume-pod
spec:
  containers:
  - name: nginx
    image: nginx
    volumeMounts:
    - mountPath: /data
      name: data-volume
  volumes:
  - name: data-volume
    emptyDir: {}
```

---

**10. Task:**  
Troubleshoot a Pod stuck in `Pending` state.  

**Answer:**  
1. Check Pod description for events:  
   ```bash
   kubectl describe pod <pod-name>
   ```  
2. Common causes:  
   - **Insufficient resources:** Update the node's resource limits.  
   - **Missing volume:** Verify volume claims.  
3. Reschedule by deleting and updating configuration.  

---

### **Configuration Management**

**11. Task:**  
Create a Secret named `db-secret` with the key `password=MySecretPassword`. Use this Secret in a Pod as an environment variable.  

**Answer:**  
```yaml
# Secret
apiVersion: v1
kind: Secret
metadata:
  name: db-secret
type: Opaque
data:
  password: bXlTZWNyZXRQYXNzd29yZA== # Base64 encoded

# Pod
apiVersion: v1
kind: Pod
metadata:
  name: app-with-secret
spec:
  containers:
  - name: app-container
    image: nginx
    env:
    - name: DB_PASSWORD
      valueFrom:
        secretKeyRef:
          name: db-secret
          key: password
```
---

### **Observability and Debugging**

**12. Task:**  
Debug a Pod stuck in `CrashLoopBackOff` state.  

**Answer:**  
1. Check Pod logs:  
   ```bash
   kubectl logs <pod-name>
   ```  
2. Use `kubectl describe` to check events and configurations:  
   ```bash
   kubectl describe pod <pod-name>
   ```  
3. Troubleshoot:  
   - Fix incorrect commands in the Pod spec.  
   - Check for missing ConfigMaps or Secrets.  
   - Ensure resource limits are adequate.  

---

**13. Task:**  
Create a Liveness Probe for a Pod named `web-pod` to check if the application is healthy every 10 seconds. Use HTTP GET on `/healthz`.  

**Answer:**  
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: web-pod
spec:
  containers:
  - name: nginx
    image: nginx
    livenessProbe:
      httpGet:
        path: /healthz
        port: 80
      initialDelaySeconds: 3
      periodSeconds: 10
```

---

**14. Task:**  
Monitor the logs of a Pod named `app-logs` and filter logs containing the word `error`.  

**Answer:**  
```bash
kubectl logs app-logs | grep "error"
```

---

### **State Persistence**

**15. Task:**  
Create a PersistentVolume (PV) named `app-pv` with 5Gi capacity, access mode `ReadWriteOnce`, and hostPath `/mnt/data`. Create a PersistentVolumeClaim (PVC) named `app-pvc` to use this PV.  

**Answer:**  
```yaml
# PersistentVolume
apiVersion: v1
kind: PersistentVolume
metadata:
  name: app-pv
spec:
  capacity:
    storage: 5Gi
  accessModes:
  - ReadWriteOnce
  hostPath:
    path: /mnt/data

# PersistentVolumeClaim
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: app-pvc
spec:
  accessModes:
  - ReadWriteOnce
  resources:
    requests:
      storage: 5Gi
```

---

**16. Task:**  
Attach the above PersistentVolumeClaim `app-pvc` to a Pod named `app-pod` at `/data`.  

**Answer:**  
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: app-pod
spec:
  containers:
  - name: nginx
    image: nginx
    volumeMounts:
    - mountPath: /data
      name: app-storage
  volumes:
  - name: app-storage
    persistentVolumeClaim:
      claimName: app-pvc
```

---

### **Scheduling and Affinity**

**17. Task:**  
Create a Pod named `web-app` with the following node affinity:  
- The Pod should only run on nodes labeled `disk=ssd`.  

**Answer:**  
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: web-app
spec:
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: disk
            operator: In
            values:
            - ssd
  containers:
  - name: nginx
    image: nginx
```

---

**18. Task:**  
Taint a node named `node-1` to allow only Pods with tolerations for the key `key1` and value `value1`.  

**Answer:**  
1. Taint the node:  
   ```bash
   kubectl taint nodes node-1 key1=value1:NoSchedule
   ```  

2. Create a Pod with a toleration:  
   ```yaml
   apiVersion: v1
   kind: Pod
   metadata:
     name: toleration-pod
   spec:
     tolerations:
     - key: "key1"
       operator: "Equal"
       value: "value1"
       effect: "NoSchedule"
     containers:
     - name: nginx
       image: nginx
   ```

---

### **Networking**

**19. Task:**  
Create a NetworkPolicy named `allow-web` to allow Pods with the label `app=frontend` in namespace `web-namespace` to connect to port `8080` of Pods labeled `app=backend`.  

**Answer:**  
```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-web
  namespace: web-namespace
spec:
  podSelector:
    matchLabels:
      app: backend
  ingress:
  - from:
    - podSelector:
        matchLabels:
          app: frontend
    ports:
    - protocol: TCP
      port: 8080
```

---

**20. Task:**  
Expose a Deployment named `api-deploy` using an Ingress with the following rules:  
- Host: `api.example.com`  
- Path: `/api`  
- Backend Service: `api-service`  

**Answer:**  
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: api-ingress
spec:
  rules:
  - host: api.example.com
    http:
      paths:
      - path: /api
        pathType: Prefix
        backend:
          service:
            name: api-service
            port:
              number: 80
```

---

### **Rollout Management**

**21. Task:**  
Perform a rolling update on the Deployment `nginx-deploy` to update the image from `nginx:1.17` to `nginx:1.18` with a max surge of `2` and max unavailable set to `1`.  

**Answer:**  
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deploy
spec:
  replicas: 3
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 2
      maxUnavailable: 1
  template:
    spec:
      containers:
      - name: nginx
        image: nginx:1.18
```

**Command Alternative:**  
```bash
kubectl set image deployment/nginx-deploy nginx=nginx:1.18
kubectl rollout status deployment/nginx-deploy
```

---

### **Application Design and Management**

**22. Task:**  
Create a Kubernetes service of type `ClusterIP` for the `nginx-deploy` application.

**Answer:**  
```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  selector:
    app: nginx
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: ClusterIP
```

---

**23. Task:**  
Create a StatefulSet for a Redis database with a persistent volume claim for each replica. The Redis image should be `redis:alpine`.

**Answer:**  
```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: redis-statefulset
spec:
  serviceName: "redis"
  replicas: 3
  selector:
    matchLabels:
      app: redis
  template:
    metadata:
      labels:
        app: redis
    spec:
      containers:
      - name: redis
        image: redis:alpine
        volumeMounts:
        - name: redis-data
          mountPath: /data
  volumeClaimTemplates:
  - metadata:
      name: redis-data
    spec:
      accessModes: [ "ReadWriteOnce" ]
      resources:
        requests:
          storage: 1Gi
```

---

### **Scheduling and Affinity**

**24. Task:**  
Create a Pod that will run on a node with the label `disk=ssd` using node affinity.

**Answer:**  
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: app-pod
spec:
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: disk
            operator: In
            values:
            - ssd
  containers:
  - name: nginx
    image: nginx
```

---

**25. Task:**  
Schedule a Pod to run only on nodes with more than 4 CPUs.

**Answer:**  
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: app-pod
spec:
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: kubernetes.io/hostname
            operator: In
            values:
            - node-1
            - node-2
  containers:
  - name: nginx
    image: nginx
```

---

### **Services and Networking**

**26. Task:**  
Create a service of type `NodePort` for the deployment `nginx-deploy` to expose the service externally.

**Answer:**  
```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-nodeport-service
spec:
  selector:
    app: nginx
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
      nodePort: 30001
  type: NodePort
```

---

**27. Task:**  
Create a Kubernetes Ingress resource for routing HTTP requests to a service named `api-service` based on the path `/api`.

**Answer:**  
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: api-ingress
spec:
  rules:
  - host: api.example.com
    http:
      paths:
      - path: /api
        pathType: Prefix
        backend:
          service:
            name: api-service
            port:
              number: 80
```

---

### **Monitoring and Logging**

**28. Task:**  
Create a Pod that runs the `fluentd` container to collect logs from other containers in the cluster.

**Answer:**  
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: fluentd-pod
spec:
  containers:
  - name: fluentd
    image: fluent/fluentd:v1.13-1
    ports:
    - containerPort: 24224
    volumeMounts:
    - name: fluentd-config
      mountPath: /fluentd/etc
  volumes:
  - name: fluentd-config
    configMap:
      name: fluentd-config
```

---

**29. Task:**  
Monitor the health of the `nginx-deploy` using a liveness probe that checks the `/healthz` endpoint every 15 seconds.

**Answer:**  
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deploy
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
        image: nginx:latest
        livenessProbe:
          httpGet:
            path: /healthz
            port: 80
          initialDelaySeconds: 3
          periodSeconds: 15
```

---

### **Resource Requests and Limits**

**30. Task:**  
Set resource requests and limits for the `nginx` container to ensure it gets at least 100Mi of memory and 200m of CPU but doesn't exceed 200Mi of memory and 500m of CPU.

**Answer:**  
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deploy
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
        image: nginx:latest
        resources:
          requests:
            memory: "100Mi"
            cpu: "200m"
          limits:
            memory: "200Mi"
            cpu: "500m"
```

---

### **Scaling and Auto-scaling**

**31. Task:**  
Scale the `nginx-deploy` Deployment to 5 replicas manually.

**Answer:**  
```bash
kubectl scale deployment nginx-deploy --replicas=5
```

---

**32. Task:**  
Create an HPA (Horizontal Pod Autoscaler) for the `nginx-deploy` Deployment to scale the replicas between 1 and 10 based on CPU utilization.

**Answer:**  
```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: nginx-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: nginx-deploy
  minReplicas: 1
  maxReplicas: 10
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: AverageValue
        averageValue: "200m"
```

---

### **Security and RBAC**

**33. Task:**  
Create a ClusterRole that allows access to `Pod` and `Service` resources in the `default` namespace.

**Answer:**  
```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: pod-service-access
rules:
- apiGroups: [""]
  resources: ["pods", "services"]
  verbs: ["get", "list"]
```

---

**34. Task:**  
Create a ServiceAccount named `admin-sa` and bind the above `pod-service-access` ClusterRole to this account.

**Answer:**  
```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: admin-sa
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: admin-sa-binding
subjects:
- kind: ServiceAccount
  name: admin-sa
  namespace: default
roleRef:
  kind: ClusterRole
  name: pod-service-access
  apiGroup: rbac.authorization.k8s.io
```

---

### **Storage and Volumes**

**35. Task:**  
Create a persistent volume claim named `pvc-1` with `ReadWriteOnce` access mode and 1Gi storage for a Pod.

**Answer:**  
```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pvc-1
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
---
apiVersion: v1
kind: Pod
metadata:
  name: app-pod
spec:
  containers:
  - name: nginx
    image: nginx
    volumeMounts:
    - mountPath: /usr/share/nginx/html
      name: pvc-storage
  volumes:
  - name: pvc-storage
    persistentVolumeClaim:
      claimName: pvc-1
```

---



