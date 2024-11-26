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

**24. Task:**  
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

**25. Task:**  
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

### **Monitoring and Logging**

**26. Task:**  
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

### **Resource Requests and Limits**

**27. Task:**  
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

**28. Task:**  
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

**29. Task:**  
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

**30. Task:**  
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
**31. Task:**  
Configure a `ConfigMap` to store application configuration data and inject it into a Pod.

**Answer:**  
Create the ConfigMap:  
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  APP_ENV: "production"
  APP_DEBUG: "false"
```

Use the ConfigMap in a Pod:  
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: app-pod
spec:
  containers:
  - name: app-container
    image: nginx
    env:
    - name: APP_ENV
      valueFrom:
        configMapKeyRef:
          name: app-config
          key: APP_ENV
    - name: APP_DEBUG
      valueFrom:
        configMapKeyRef:
          name: app-config
          key: APP_DEBUG
```

---

**32. Task:**  
Secure sensitive application data using a Secret and inject it into a Pod.

**Answer:**  
Create the Secret:  
```yaml
apiVersion: v1
kind: Secret
metadata:
  name: db-credentials
type: Opaque
data:
  username: bXl1c2VybmFtZQ== # Base64 encoded "myusername"
  password: bXlwYXNzd29yZA== # Base64 encoded "mypassword"
```

Use the Secret in a Pod:  
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: db-pod
spec:
  containers:
  - name: db-container
    image: mysql
    env:
    - name: DB_USER
      valueFrom:
        secretKeyRef:
          name: db-credentials
          key: username
    - name: DB_PASSWORD
      valueFrom:
        secretKeyRef:
          name: db-credentials
          key: password
```

---

### **Multi-Container Pods**

**33. Task:**  
Create a multi-container Pod where one container serves as the main application and the other as a logging sidecar.

**Answer:**  
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: multi-container-pod
spec:
  containers:
  - name: app-container
    image: nginx
    volumeMounts:
    - name: shared-logs
      mountPath: /var/log/app
  - name: logging-container
    image: busybox
    command: ["/bin/sh", "-c", "tail -f /var/log/app/access.log"]
    volumeMounts:
    - name: shared-logs
      mountPath: /var/log/app
  volumes:
  - name: shared-logs
    emptyDir: {}
```

---

### **Working with Jobs and CronJobs**

**34. Task:**  
Create a Kubernetes Job to run a one-time batch process using the `busybox` image.

**Answer:**  
```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: batch-job
spec:
  template:
    spec:
      containers:
      - name: batch-container
        image: busybox
        command: ["sh", "-c", "echo Hello, Kubernetes! && sleep 30"]
      restartPolicy: Never
```

---

**35. Task:**  
Create a CronJob that runs the above batch process every day at midnight.

**Answer:**  
```yaml
apiVersion: batch/v1
kind: CronJob
metadata:
  name: daily-job
spec:
  schedule: "0 0 * * *"
  jobTemplate:
    spec:
      template:
        spec:
          containers:
          - name: batch-container
            image: busybox
            command: ["sh", "-c", "echo Hello, Kubernetes! && sleep 30"]
          restartPolicy: Never
```

---

### **Application Scaling**

**36. Task:**  
Create a Kubernetes Deployment and scale it dynamically based on memory usage.

**Answer:**  
Create the Deployment:  
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: memory-intensive-app
spec:
  replicas: 2
  selector:
    matchLabels:
      app: memory-intensive-app
  template:
    metadata:
      labels:
        app: memory-intensive-app
    spec:
      containers:
      - name: app-container
        image: busybox
        command: ["sh", "-c", "while true; do echo Hello Kubernetes; sleep 10; done"]
        resources:
          limits:
            memory: "256Mi"
          requests:
            memory: "128Mi"
```

Create the HPA:  
```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: memory-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: memory-intensive-app
  minReplicas: 2
  maxReplicas: 10
  metrics:
  - type: Resource
    resource:
      name: memory
      target:
        type: Utilization
        averageUtilization: 80
```

---

**37. Task:**  
Manually scale a StatefulSet to handle increased workloads during peak hours.

**Answer:**  
```bash
kubectl scale statefulset redis-statefulset --replicas=5
```

---

### **Advanced Resource Management**

**38. Task:**  
Reserve a specific node for critical workloads by tainting it and scheduling Pods with tolerations.

**Answer:**  
Taint the node:  
```bash
kubectl taint nodes critical-node key=value:NoSchedule
```

Schedule a Pod with tolerations:  
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: critical-pod
spec:
  tolerations:
  - key: "key"
    operator: "Equal"
    value: "value"
    effect: "NoSchedule"
  containers:
  - name: nginx
    image: nginx
```

---

**39. Task:**  
Create a ResourceQuota to limit the number of Pods and CPU usage in a namespace.

**Answer:**  
```yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: namespace-quota
  namespace: my-namespace
spec:
  hard:
    pods: "10"
    limits.cpu: "4"
```

---

### **CKAD Real-Time Issues**

**40. Task:**  
Identify and fix a misconfigured Pod that cannot communicate with the Service.

**Solution:**  
- Verify the Service `selector` matches the Pod `labels`.
- Check the Service and Pod are in the same namespace.
- Use `kubectl describe service <service-name>` and `kubectl logs` for debugging.

---
