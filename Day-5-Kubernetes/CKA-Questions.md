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

