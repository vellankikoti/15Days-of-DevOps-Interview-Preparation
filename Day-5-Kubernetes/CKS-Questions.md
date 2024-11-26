### **Certified Kubernetes Security Specialist (CKS) Guide**  

The following questions are designed to test and enhance your knowledge of Kubernetes security. These scenarios cover authentication, authorization, secrets management, network policies, runtime security, and more.

---

### **Cluster Security**

**1. Task:**  
Harden the Kubernetes API server by enabling audit logging.

**Answer:**  
Update the API server configuration file (e.g., `/etc/kubernetes/manifests/kube-apiserver.yaml`):  
```yaml
- --audit-log-path=/var/log/kubernetes/audit.log
- --audit-policy-file=/etc/kubernetes/audit-policy.yaml
```

Create an audit policy file `/etc/kubernetes/audit-policy.yaml`:  
```yaml
apiVersion: audit.k8s.io/v1
kind: Policy
rules:
- level: Metadata
```

Restart the API server to apply changes.

---

**2. Task:**  
Restrict access to the Kubernetes API server to a specific CIDR range.

**Answer:**  
Modify the API server configuration:  
```yaml
- --authorization-mode=Node,RBAC
- --bind-address=0.0.0.0
- --client-ca-file=/etc/kubernetes/pki/ca.crt
- --tls-cert-file=/etc/kubernetes/pki/apiserver.crt
- --tls-private-key-file=/etc/kubernetes/pki/apiserver.key
- --insecure-bind-address=127.0.0.1
- --insecure-port=8080
- --kubelet-preferred-address-types=InternalIP,Hostname,ExternalIP
- --kubelet-client-certificate=/etc/kubernetes/pki/apiserver-kubelet-client.crt
- --kubelet-client-key=/etc/kubernetes/pki/apiserver-kubelet-client.key
- --allow-privileged=true
- --advertise-address=10.0.0.1
- --etcd-servers=https://127.0.0.1:2379
- --etcd-cafile=/etc/kubernetes/pki/etcd/ca.crt
- --etcd-certfile=/etc/kubernetes/pki/apiserver-etcd-client.crt
- --etcd-keyfile=/etc/kubernetes/pki/apiserver-etcd-client.key
- --enable-admission-plugins=NodeRestriction
- --service-cluster-ip-range=10.96.0.0/12
- --client-ca-file=/etc/kubernetes/pki/ca.crt
- --authorization-mode=Node,RBAC
```

---

### **Network Security**

**3. Task:**  
Create a NetworkPolicy that denies all ingress traffic except for Pods with the label `app=frontend` on port 80.

**Answer:**  
```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-frontend
  namespace: default
spec:
  podSelector:
    matchLabels:
      app: frontend
  ingress:
  - from:
    - podSelector:
        matchLabels:
          app: frontend
    ports:
    - protocol: TCP
      port: 80
```

---

**4. Task:**  
Block all egress traffic from a specific namespace.

**Answer:**  
```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: deny-egress
  namespace: restricted-namespace
spec:
  podSelector: {}
  policyTypes:
  - Egress
  egress: []
```

---

### **Secrets Management**

**5. Task:**  
Encrypt Kubernetes Secrets at rest.

**Answer:**  
Edit the `kube-apiserver.yaml` file to include the encryption configuration:  
```yaml
- --encryption-provider-config=/etc/kubernetes/encryption-config.yaml
```

Create `/etc/kubernetes/encryption-config.yaml`:  
```yaml
apiVersion: apiserver.config.k8s.io/v1
kind: EncryptionConfiguration
resources:
- resources:
  - secrets
  providers:
  - aescbc:
      keys:
      - name: key1
        secret: <base64-encoded-key>
  - identity: {}
```

Restart the API server.

---

### **Authentication and Authorization**

**6. Task:**  
Configure a Role to allow a specific user to view Pods in a namespace.

**Answer:**  
Create a Role:  
```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: dev-team
  name: pod-reader
rules:
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["get", "list", "watch"]
```

Bind the Role to a user:  
```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: pod-reader-binding
  namespace: dev-team
subjects:
- kind: User
  name: johndoe
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role
  name: pod-reader
  apiGroup: rbac.authorization.k8s.io
```

---

**7. Task:**  
Ensure that a specific ServiceAccount can only access ConfigMaps.

**Answer:**  
Create a Role and bind it to the ServiceAccount:  
```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: config-team
  name: configmap-access
rules:
- apiGroups: [""]
  resources: ["configmaps"]
  verbs: ["get", "list", "watch"]
---
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: configmap-access-binding
  namespace: config-team
subjects:
- kind: ServiceAccount
  name: config-sa
  namespace: config-team
roleRef:
  kind: Role
  name: configmap-access
  apiGroup: rbac.authorization.k8s.io
```

---

### **Runtime Security**

**8. Task:**  
Prevent containers from running as root.

**Answer:**  
Create a PodSecurityPolicy:  
```yaml
apiVersion: policy/v1beta1
kind: PodSecurityPolicy
metadata:
  name: non-root-policy
spec:
  runAsUser:
    rule: MustRunAsNonRoot
```

---

### **Advanced Security Scenarios**

**9. Task:**  
Rotate TLS certificates in a Kubernetes cluster.

**Answer:**  
1. Backup the current certificates.
2. Generate new certificates.
3. Replace certificates in `/etc/kubernetes/pki/`.
4. Restart control plane components to apply the changes.

---

**10. Task:**  
Ensure an application can only access a specific set of environment variables.

**Answer:**  
Use a ConfigMap to define allowed variables and mount it into the Pod.  

---

### **11. Task:** Ensure containers run with the least privilege principle.

**Answer:**  
Use a PodSecurityContext and ContainerSecurityContext in the Pod specification:  
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: least-privilege
spec:
  securityContext:
    runAsNonRoot: true
    fsGroup: 2000
  containers:
  - name: app
    image: nginx
    securityContext:
      runAsUser: 1000
      capabilities:
        drop:
        - ALL
```

---

### **12. Task:** Restrict access to Kubernetes secrets to a specific namespace.

**Answer:**  
Create a Role and RoleBinding scoped to a namespace:  
```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: my-namespace
  name: secrets-reader
rules:
- apiGroups: [""]
  resources: ["secrets"]
  verbs: ["get", "list", "watch"]
---
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: secrets-reader-binding
  namespace: my-namespace
subjects:
- kind: ServiceAccount
  name: my-service-account
  namespace: my-namespace
roleRef:
  kind: Role
  name: secrets-reader
  apiGroup: rbac.authorization.k8s.io
```

---

### **13. Task:** Create a Pod that mounts a ConfigMap as an environment variable.

**Answer:**  
1. Create a ConfigMap:  
```bash
kubectl create configmap app-config --from-literal=ENV=production
```

2. Use the ConfigMap in a Pod:  
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: config-env-pod
spec:
  containers:
  - name: app
    image: nginx
    env:
    - name: ENV
      valueFrom:
        configMapKeyRef:
          name: app-config
          key: ENV
```

---

### **14. Task:** Detect privilege escalation in running containers.

**Answer:**  
Use tools like Falco to monitor container activity:  
1. Install Falco:  
```bash
helm repo add falcosecurity https://falcosecurity.github.io/charts
helm install falco falcosecurity/falco
```

2. Falco will log alerts for privilege escalation.

---

### **15. Task:** Create an Admission Controller to block privileged Pods.

**Answer:**  
1. Enable admission plugins in the API server:  
   Add `PodSecurityPolicy` to `--enable-admission-plugins` in the `kube-apiserver.yaml`.

2. Create a PodSecurityPolicy:  
```yaml
apiVersion: policy/v1beta1
kind: PodSecurityPolicy
metadata:
  name: restricted
spec:
  privileged: false
  runAsUser:
    rule: MustRunAsNonRoot
```

3. Bind the policy to users or ServiceAccounts via RBAC.

---

### **16. Task:** Create a NetworkPolicy to allow ingress traffic only on port 443.

**Answer:**  
```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-https
  namespace: my-namespace
spec:
  podSelector:
    matchLabels:
      app: web
  policyTypes:
  - Ingress
  ingress:
  - from:
    - podSelector:
        matchLabels:
          role: frontend
    ports:
    - protocol: TCP
      port: 443
```

---

### **17. Task:** Set up monitoring for API server audit logs.

**Answer:**  
1. Configure the API server to log audits:  
   Update the `kube-apiserver.yaml` file:  
```yaml
- --audit-log-path=/var/log/audit.log
- --audit-policy-file=/etc/kubernetes/audit-policy.yaml
```

2. Use Fluentd or another logging agent to collect and send logs to a central location.

---

### **18. Task:** Enforce image policies to allow only signed images.

**Answer:**  
Use tools like OPA/Gatekeeper:  
1. Install Gatekeeper:  
```bash
kubectl apply -f https://raw.githubusercontent.com/open-policy-agent/gatekeeper/master/deploy/gatekeeper.yaml
```

2. Create a policy:  
```yaml
apiVersion: constraints.gatekeeper.sh/v1beta1
kind: K8sRequiredLabels
metadata:
  name: signed-images-only
spec:
  match:
    kinds:
    - apiGroups: [""]
      kinds: ["Pod"]
  parameters:
    requiredLabels:
    - signed
```

---

### **19. Task:** Rotate all Kubernetes certificates.

**Answer:**  
1. Backup existing certificates.  
2. Use `kubeadm` to renew certificates:  
```bash
kubeadm alpha certs renew all
```

3. Restart control plane components.

---

### **20. Task:** Monitor CPU and memory usage of all Pods.

**Answer:**  
1. Use Prometheus and Grafana.  
2. Example Prometheus query for CPU usage:  
```promql
sum(rate(container_cpu_usage_seconds_total[1m])) by (pod, namespace)
```

3. Visualize data in Grafana dashboards.

---

### **21. Task:** Create a Kubernetes Secret and use it in a Pod.

**Answer:**  
1. Create a Secret:  
```bash
kubectl create secret generic db-credentials --from-literal=username=admin --from-literal=password=securepassword
```

2. Use the Secret in a Pod:  
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: secret-pod
spec:
  containers:
  - name: app
    image: nginx
    env:
    - name: DB_USER
      valueFrom:
        secretKeyRef:
          name: db-credentials
          key: username
    - name: DB_PASS
      valueFrom:
        secretKeyRef:
          name: db-credentials
          key: password
```

---

### **22. Task:** Restrict API server access to a specific IP range.

**Answer:**  
1. Update `kube-apiserver.yaml`:  
```yaml
- --authorization-mode=Node,RBAC
- --bind-address=0.0.0.0
- --client-ca-file=/etc/kubernetes/pki/ca.crt
- --tls-cert-file=/etc/kubernetes/pki/apiserver.crt
- --tls-private-key-file=/etc/kubernetes/pki/apiserver.key
- --advertise-address=<API-SERVER-IP>
- --allow-privileged=true
- --audit-log-path=/var/log/audit.log
- --etcd-servers=https://127.0.0.1:2379
- --etcd-cafile=/etc/kubernetes/pki/etcd/ca.crt
- --etcd-certfile=/etc/kubernetes/pki/apiserver-etcd-client.crt
- --etcd-keyfile=/etc/kubernetes/pki/apiserver-etcd-client.key
- --service-cluster-ip-range=10.96.0.0/12
- --token-auth-file=/etc/kubernetes/pki/tokens.csv
```
2. Restart the API server.

---

### **23. Task:** Create a Pod with read-only root file system.

**Answer:**  
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: read-only-pod
spec:
  containers:
  - name: app
    image: nginx
    securityContext:
      readOnlyRootFilesystem: true
      runAsNonRoot: true
      capabilities:
        drop:
        - ALL
```

---

### **24. Task:** Scan container images for vulnerabilities.

**Answer:**  
Use tools like **Trivy**:  
1. Install Trivy:  
```bash
brew install trivy
```

2. Scan an image:  
```bash
trivy image nginx:latest
```

---

### **25. Task:** Enforce Pod-level resource quotas in a namespace.

**Answer:**  
1. Create a ResourceQuota:  
```yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: pod-quota
  namespace: my-namespace
spec:
  hard:
    pods: "5"
```

2. Apply the ResourceQuota:  
```bash
kubectl apply -f pod-quota.yaml
```

---

### **26. Task:** Isolate workloads using network segmentation.

**Answer:**  
Create a NetworkPolicy:  
```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: isolate-workloads
  namespace: app
spec:
  podSelector:
    matchLabels:
      role: database
  policyTypes:
  - Ingress
  ingress:
  - from:
    - podSelector:
        matchLabels:
          role: frontend
    ports:
    - protocol: TCP
      port: 5432
```

---

### **27. Task:** Protect etcd data using encryption.

**Answer:**  
1. Enable etcd encryption in the Kubernetes cluster:  
   Update the `encryption-config.yaml` file:  
```yaml
apiVersion: apiserver.config.k8s.io/v1
kind: EncryptionConfiguration
resources:
- resources:
  - secrets
  providers:
  - aescbc:
      keys:
      - name: key1
        secret: dGVzdC1rZXktd2l0aC1zZWNyZXQ=
  - identity: {}
```

2. Update the API server to use the encryption config:  
```bash
--encryption-provider-config=/etc/kubernetes/encryption-config.yaml
```

3. Restart the API server.

---

### **28. Task:** Rotate Kubernetes secrets automatically.

**Answer:**  
Use tools like **External Secrets Operator**:  
1. Install the operator:  
```bash
kubectl apply -f https://github.com/external-secrets/external-secrets/releases/latest/download/install.yaml
```

2. Create an ExternalSecret resource:  
```yaml
apiVersion: external-secrets.io/v1alpha1
kind: ExternalSecret
metadata:
  name: my-secret
spec:
  refreshInterval: 1h
  secretStoreRef:
    name: aws-secret-store
    kind: SecretStore
  target:
    name: my-secret
  data:
    - secretKey: username
      remoteRef:
        key: /path/to/secret
```

---

### **29. Task:** Audit all API requests to identify potential risks.

**Answer:**  
1. Enable audit logging in the API server.  
2. Use a policy to filter high-risk events:  
```yaml
apiVersion: audit.k8s.io/v1
kind: Policy
rules:
- level: RequestResponse
  verbs: ["create", "update", "delete"]
  resources:
  - group: ""
    resources: ["pods"]
```

3. Collect logs using Fluentd and analyze with Elasticsearch/Kibana.

---

### **30. Task:** Harden Kubernetes by enforcing NodeRestriction.

**Answer:**  
Add the `NodeRestriction` admission controller to the API server:  
1. Update the `kube-apiserver.yaml` file:  
```bash
--enable-admission-plugins=NodeRestriction
```

2. Restart the API server.  

---

### **31. Task:** Enforce namespace-level isolation for all workloads.

**Answer:**  
1. Create a default NetworkPolicy for the namespace:  
```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: default-deny
  namespace: secure-namespace
spec:
  podSelector: {}
  policyTypes:
  - Ingress
  - Egress
```

2. Apply the policy:  
```bash
kubectl apply -f default-deny.yaml
```

---

### **32. Task:** Set up a Pod Security Policy to enforce non-root user deployment.

**Answer:**  
1. Create a PodSecurityPolicy:  
```yaml
apiVersion: policy/v1beta1
kind: PodSecurityPolicy
metadata:
  name: restrict-root
spec:
  privileged: false
  runAsUser:
    rule: MustRunAsNonRoot
  seLinux:
    rule: RunAsAny
  supplementalGroups:
    rule: RunAsAny
  fsGroup:
    rule: RunAsAny
```

2. Bind the policy to a role:  
```bash
kubectl create clusterrole psp:restrict-root --verb=use --resource=podsecuritypolicies --resource-name=restrict-root
kubectl create clusterrolebinding restrict-root-binding --clusterrole=psp:restrict-root --group=system:authenticated
```

---

### **33. Task:** Configure Kubernetes to use OpenID Connect (OIDC) for authentication.

**Answer:**  
1. Update `kube-apiserver.yaml`:  
```bash
--oidc-issuer-url=https://accounts.google.com
--oidc-client-id=YOUR-CLIENT-ID
--oidc-username-claim=email
--oidc-groups-claim=groups
```

2. Restart the API server.  
3. Test authentication with the OIDC token.

---

### **34. Task:** Secure API server communications using mutual TLS.

**Answer:**  
1. Generate a certificate for the client using OpenSSL:  
```bash
openssl genrsa -out client.key 2048
openssl req -new -key client.key -out client.csr -subj "/CN=my-client"
openssl x509 -req -in client.csr -CA ca.crt -CAkey ca.key -CAcreateserial -out client.crt -days 365
```

2. Configure the API server to accept the certificate:  
   Add `--client-ca-file=/etc/kubernetes/pki/ca.crt` to `kube-apiserver.yaml`.

---

### **35. Task:** Enable audit logging for security monitoring.

**Answer:**  
1. Create an audit policy file:  
```yaml
apiVersion: audit.k8s.io/v1
kind: Policy
rules:
- level: Metadata
  verbs: ["create", "update", "delete"]
  resources:
  - group: ""
    resources: ["pods", "deployments"]
```

2. Add the policy to the API server:  
```bash
--audit-policy-file=/etc/kubernetes/audit-policy.yaml
--audit-log-path=/var/log/kube-audit.log
```

---

### **36. Task:** Use Falco for runtime security monitoring.

**Answer:**  
1. Install Falco:  
```bash
kubectl apply -f https://raw.githubusercontent.com/falcosecurity/charts/master/deploy/kubernetes/falco.yaml
```

2. Monitor suspicious activities, such as process execution in containers:  
```bash
falco --list
```

---

### **37. Task:** Implement node hardening by restricting SSH access.

**Answer:**  
1. Disable root SSH access in `/etc/ssh/sshd_config`:  
```bash
PermitRootLogin no
```

2. Restart the SSH service:  
```bash
systemctl restart sshd
```

3. Use jump hosts or bastion hosts for SSH access.

---

### **38. Task:** Prevent Pods from escalating privileges.

**Answer:**  
Set security context in the Pod spec:  
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: no-escalate
spec:
  containers:
  - name: app
    image: nginx
    securityContext:
      allowPrivilegeEscalation: false
```

---

### **39. Task:** Create a PersistentVolume with access limited to a specific namespace.

**Answer:**  
1. Create the PersistentVolume:  
```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-limited
spec:
  capacity:
    storage: 1Gi
  accessModes:
  - ReadWriteOnce
  persistentVolumeReclaimPolicy: Retain
  storageClassName: manual
  hostPath:
    path: /mnt/data
```

2. Bind it to a PersistentVolumeClaim in the specific namespace:  
```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pvc-limited
  namespace: my-namespace
spec:
  accessModes:
  - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
  storageClassName: manual
```

---

### **40. Task:** Configure kubelet authentication with client certificates.

**Answer:**  
1. Generate a client certificate for the kubelet:  
```bash
openssl genrsa -out kubelet-client.key 2048
openssl req -new -key kubelet-client.key -out kubelet-client.csr -subj "/CN=kubelet"
openssl x509 -req -in kubelet-client.csr -CA ca.crt -CAkey ca.key -CAcreateserial -out kubelet-client.crt -days 365
```

2. Configure the kubelet to use the certificate in `/var/lib/kubelet/config.yaml`:  
```yaml
authentication:
  x509:
    clientCAFile: /etc/kubernetes/pki/ca.crt
```

---

### **41. Task:** Restrict access to the Kubernetes Dashboard using RBAC.

**Answer:**  
1. Create a minimal-permission Role for Dashboard access:  
```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: kubernetes-dashboard
  name: dashboard-read-only
rules:
- apiGroups: [""]
  resources: ["pods", "services", "deployments"]
  verbs: ["get", "list"]
```

2. Bind the Role to a specific user or group:  
```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: dashboard-read-only-binding
  namespace: kubernetes-dashboard
subjects:
- kind: User
  name: dashboard-user
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role
  name: dashboard-read-only
  apiGroup: rbac.authorization.k8s.io
```

3. Apply the manifests:  
```bash
kubectl apply -f role.yaml -f rolebinding.yaml
```

---

### **42. Task:** Configure etcd to use encryption for all stored secrets.

**Answer:**  
1. Create an encryption configuration file:  
```yaml
apiVersion: apiserver.config.k8s.io/v1
kind: EncryptionConfiguration
resources:
  - resources:
      - secrets
    providers:
      - aescbc:
          keys:
            - name: key1
              secret: c2VjcmV0LWtleS1zdHJpbmc=
      - identity: {}
```

2. Update the `kube-apiserver` manifest to use the encryption config file:  
```bash
--encryption-provider-config=/etc/kubernetes/encryption-config.yaml
```

3. Restart the API server:  
```bash
systemctl restart kubelet
```

---

### **43. Task:** Create a Pod with a read-only root filesystem for increased security.

**Answer:**  
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: secure-app
spec:
  containers:
  - name: app
    image: nginx
    securityContext:
      readOnlyRootFilesystem: true
      runAsNonRoot: true
```

---

### **44. Task:** Implement a PodDisruptionBudget to ensure availability during maintenance.

**Answer:**  
1. Create the PodDisruptionBudget:  
```yaml
apiVersion: policy/v1
kind: PodDisruptionBudget
metadata:
  name: app-pdb
spec:
  minAvailable: 2
  selector:
    matchLabels:
      app: my-app
```

2. Apply the manifest:  
```bash
kubectl apply -f pdb.yaml
```

---

### **45. Task:** Enable audit logging for privileged operations.

**Answer:**  
1. Create an audit policy file:  
```yaml
apiVersion: audit.k8s.io/v1
kind: Policy
rules:
- level: RequestResponse
  verbs: ["create", "delete"]
  resources:
  - group: ""
    resources: ["pods/exec", "secrets"]
```

2. Add the policy to the API server:  
```bash
--audit-policy-file=/etc/kubernetes/audit-policy.yaml
--audit-log-path=/var/log/kubernetes/audit.log
```

3. Restart the API server.

---

### **46. Task:** Prevent accidental deletion of critical resources.

**Answer:**  
Use finalizers in your resource definitions:  
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: critical-app
  finalizers:
    - foregroundDeletion
spec:
  replicas: 2
  selector:
    matchLabels:
      app: critical-app
  template:
    metadata:
      labels:
        app: critical-app
    spec:
      containers:
      - name: app
        image: nginx
```

---

### **47. Task:** Set up namespace-level resource quotas.

**Answer:**  
1. Create a ResourceQuota for the namespace:  
```yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: resource-quota
  namespace: dev-team
spec:
  hard:
    pods: "10"
    requests.cpu: "4"
    limits.memory: "8Gi"
```

2. Apply the manifest:  
```bash
kubectl apply -f resource-quota.yaml
```

---

### **48. Task:** Limit container resource usage to avoid over-provisioning.

**Answer:**  
1. Define resource requests and limits in the container spec:  
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: limited-resource-pod
spec:
  containers:
  - name: app
    image: nginx
    resources:
      requests:
        memory: "64Mi"
        cpu: "250m"
      limits:
        memory: "128Mi"
        cpu: "500m"
```

---

### **49. Task:** Ensure containers run with non-root users.

**Answer:**  
1. Set the `runAsNonRoot` option in the securityContext:  
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: non-root-app
spec:
  containers:
  - name: app
    image: nginx
    securityContext:
      runAsUser: 1000
      runAsNonRoot: true
```

---

### **50. Task:** Use namespaces to logically separate workloads.

**Answer:**  
1. Create a new namespace:  
```bash
kubectl create namespace dev
```

2. Deploy resources into the namespace:  
```bash
kubectl apply -f app.yaml -n dev
```

3. List resources in the namespace:  
```bash
kubectl get all -n dev
```

---

### **51. Task:** Configure Secrets to be encrypted at rest.

**Answer:**  
1. Enable encryption for secrets in the `kube-apiserver`:  
   - Add the encryption provider flag:  
   ```bash
   --encryption-provider-config=/etc/kubernetes/encryption-config.yaml
   ```
   - Use the following encryption config:  
   ```yaml
   apiVersion: apiserver.config.k8s.io/v1
   kind: EncryptionConfiguration
   resources:
     - resources:
         - secrets
       providers:
         - aescbc:
             keys:
               - name: key1
                 secret: c2VjcmV0LWtleS1zdHJpbmc=
         - identity: {}
   ```

2. Apply the configuration and restart the API server.

---

### **52. Task:** Limit external IP usage in a namespace.

**Answer:**  
1. Create a NetworkPolicy to restrict access to external IPs:  
```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: restrict-external-ip
  namespace: production
spec:
  podSelector: {}
  policyTypes:
  - Egress
  egress:
  - to:
    - ipBlock:
        cidr: 10.0.0.0/8
    ports:
    - protocol: TCP
      port: 80
```

2. Apply the policy:  
```bash
kubectl apply -f restrict-external-ip.yaml
```

---

### **53. Task:** Harden a Kubernetes cluster by disabling anonymous access to the API server.

**Answer:**  
1. Disable anonymous access by setting the following flag in the `kube-apiserver`:  
   ```bash
   --anonymous-auth=false
   ```

2. Verify:  
   - Check for `401 Unauthorized` when accessing without authentication:  
   ```bash
   curl -k https://<api-server>:6443
   ```

---

### **54. Task:** Prevent privilege escalation in containers.

**Answer:**  
1. Use a `securityContext` in the Pod spec:  
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: no-privilege-escalation
spec:
  containers:
  - name: app
    image: nginx
    securityContext:
      allowPrivilegeEscalation: false
```

2. Validate using `kubectl describe pod`:  
   - Confirm the `allowPrivilegeEscalation` setting is false.

---

### **55. Task:** Scan container images for vulnerabilities during CI/CD.

**Answer:**  
1. Use tools like **Trivy** or **Clair** to scan images:  
   - Trivy example:  
   ```bash
   trivy image myregistry.com/myapp:latest
   ```

2. Integrate into a CI/CD pipeline:  
   - Add a scan step to a GitHub Actions workflow:  
   ```yaml
   steps:
     - name: Scan with Trivy
       uses: aquasecurity/trivy-action@master
       with:
         image-ref: myregistry.com/myapp:latest
   ```

---

### **56. Task:** Configure Pod Security Standards (PSS) for a namespace.

**Answer:**  
1. Enable Pod Security Admission:  
   - Add the flag to the `kube-apiserver`:  
   ```bash
   --enable-admission-plugins=PodSecurity
   ```

2. Label the namespace for the desired security level:  
   ```bash
   kubectl label namespace dev pod-security.kubernetes.io/enforce=baseline
   ```

3. Verify compliance:  
   - Attempt to deploy a privileged Pod; it should be blocked.

---

### **57. Task:** Rotate TLS certificates in a Kubernetes cluster.

**Answer:**  
1. Generate new certificates:  
   ```bash
   kubeadm certs renew all
   ```

2. Replace certificates on nodes:  
   ```bash
   cp /etc/kubernetes/pki/apiserver.crt <node-path>
   ```

3. Restart relevant components:  
   ```bash
   systemctl restart kubelet
   ```

4. Verify using:  
   ```bash
   openssl x509 -in /etc/kubernetes/pki/apiserver.crt -text -noout
   ```

---

### **58. Task:** Isolate workloads in the cluster using taints and tolerations.

**Answer:**  
1. Apply a taint to a node:  
   ```bash
   kubectl taint nodes node1 key=value:NoSchedule
   ```

2. Add tolerations to the Pod spec:  
   ```yaml
   apiVersion: v1
   kind: Pod
   metadata:
     name: isolated-pod
   spec:
     tolerations:
     - key: "key"
       operator: "Equal"
       value: "value"
       effect: "NoSchedule"
     containers:
     - name: app
       image: nginx
   ```

---

### **59. Task:** Prevent Pods from running as root.

**Answer:**  
1. Use a Pod Security Policy or admission controller:  
   ```yaml
   apiVersion: policy/v1beta1
   kind: PodSecurityPolicy
   metadata:
     name: no-root
   spec:
     runAsUser:
       rule: MustRunAsNonRoot
   ```

2. Validate using:  
   ```bash
   kubectl get psp
   ```

---

### **60. Task:** Enable logging for audit events in a cluster.

**Answer:**  
1. Create an audit policy file:  
   ```yaml
   apiVersion: audit.k8s.io/v1
   kind: Policy
   rules:
   - level: Metadata
     verbs: ["create", "delete"]
     resources:
     - group: ""
       resources: ["pods", "configmaps"]
   ```

2. Enable the audit policy in the API server:  
   ```bash
   --audit-policy-file=/etc/kubernetes/audit-policy.yaml
   --audit-log-path=/var/log/kubernetes/audit.log
   ```

3. Verify the logs:  
   ```bash
   tail -f /var/log/kubernetes/audit.log
   ```

---

### **61. Task:** Configure NetworkPolicies to isolate namespaces.

**Answer:**  
1. Create a default-deny policy for namespace `dev`:  
   ```yaml
   apiVersion: networking.k8s.io/v1
   kind: NetworkPolicy
   metadata:
     name: default-deny
     namespace: dev
   spec:
     podSelector: {}
     policyTypes:
     - Ingress
     - Egress
   ```

2. Allow specific ingress for web pods:  
   ```yaml
   apiVersion: networking.k8s.io/v1
   kind: NetworkPolicy
   metadata:
     name: allow-web-ingress
     namespace: dev
   spec:
     podSelector:
       matchLabels:
         app: web
     ingress:
     - from:
       - podSelector:
           matchLabels:
             role: frontend
   ```

---

### **62. Task:** Implement Role-Based Access Control (RBAC) for service account.

**Answer:**  
1. Create a ClusterRole with specific permissions:  
   ```yaml
   apiVersion: rbac.authorization.k8s.io/v1
   kind: ClusterRole
   metadata:
     # Role Name
     name: devops-admin
   rules:
   - verbs: ["get", "list", "create", "delete"]
     resources: ["pods", "deployments", "services"]
   ```

2. Bind the ClusterRole to a ServiceAccount:  
   ```yaml
   apiVersion: rbac.authorization.k8s.io/v1
   kind: ClusterRoleBinding
   metadata:
     name: devops-admin-binding
   subjects:
   - kind: ServiceAccount
     name: devops-sa
     namespace: dev-team
   roleRef:
     kind: ClusterRole
     name: devops-admin
     apiGroup: rbac.authorization.k8s.io
   ```

3. Apply and verify the access:  
   ```bash
   kubectl auth can-i list pods --as=system:serviceaccount:dev-team:devops-sa
   ```

---

### **63. Task:** Audit container image vulnerability scanning.

**Answer:**  
1. Use a container security tool like **Clair** or **Trivy** for image vulnerability scanning:
   - **Trivy example**:  
   ```bash
   trivy image myrepo/myapp:latest
   ```
   - **Clair** can be integrated into CI/CD pipelines to automatically scan container images for vulnerabilities as part of the build process.

---

### **64. Task:** Implement a Pod Security Policy (PSP) for non-root containers.

**Answer:**  
1. Create a PSP to enforce non-root containers:
   ```yaml
   apiVersion: policy/v1beta1
   kind: PodSecurityPolicy
   metadata:
     name: non-root-policy
   spec:
     runAsUser:
       rule: MustRunAsNonRoot
     fsGroup:
       rule: MustRunAs
   ```

2. Apply the PSP:  
   ```bash
   kubectl apply -f non-root-policy.yaml
   ```

3. Validate that containers in your cluster cannot run as root:  
   ```bash
   kubectl describe psp non-root-policy
   ```

---

### **65. Task:** Protect Kubernetes secrets using encryption.

**Answer:**  
1. Configure encryption at rest for Kubernetes secrets:
   - Create an encryption configuration file for `kube-apiserver`:
   ```yaml
   apiVersion: apiserver.config.k8s.io/v1
   kind: EncryptionConfiguration
   resources:
   - resources:
       - secrets
     providers:
       - aescbc:
           keys:
             - name: key1
               secret: <encrypted-key>
   ```
   - Apply the configuration by setting the encryption flag in the `kube-apiserver`:

   ```bash
   --encryption-provider-config=/etc/kubernetes/encryption-config.yaml
   ```

2. Verify that secrets are stored encrypted in the etcd database.

---

### **66. Task:** Protect a namespace using NetworkPolicies.

**Answer:**  
1. Create a NetworkPolicy that denies all traffic by default:
   ```yaml
   apiVersion: networking.k8s.io/v1
   kind: NetworkPolicy
   metadata:
     name: default-deny
     namespace: dev
   spec:
     podSelector: {}
     policyTypes:
     - Ingress
     - Egress
   ```

2. Allow traffic from the frontend namespace to the backend namespace on port 80:
   ```yaml
   apiVersion: networking.k8s.io/v1
   kind: NetworkPolicy
   metadata:
     name: allow-frontend-to-backend
     namespace: backend
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
         port: 80
   ```

---

### **67. Task:** Prevent containers from running privileged.

**Answer:**  
1. Set the `privileged` flag to `false` in your Pod specification:
   ```yaml
   apiVersion: v1
   kind: Pod
   metadata:
     name: secure-pod
   spec:
     containers:
     - name: app
       image: nginx
       securityContext:
         privileged: false
   ```

2. Apply and validate that containers cannot run with escalated privileges.

---

### **68. Task:** Integrate Kubernetes with an external identity provider.

**Answer:**  
1. Configure **OIDC** (OpenID Connect) with an identity provider (e.g., Google or AWS Cognito) for Kubernetes authentication:
   - Add the following flags to `kube-apiserver`:
   ```bash
   --oidc-issuer-url=https://accounts.google.com
   --oidc-client-id=<client-id>
   --oidc-username-claim=email
   --oidc-groups-claim=groups
   ```

2. Use `kubectl` to authenticate using the external identity provider:
   ```bash
   kubectl get pods --auth-provider=oidc
   ```

---

### **69. Task:** Limit container CPU and memory usage.

**Answer:**  
1. Add resource limits and requests in the Pod spec:
   ```yaml
   apiVersion: v1
   kind: Pod
   metadata:
     name: resource-limited-pod
   spec:
     containers:
     - name: app
       image: nginx
       resources:
         requests:
           memory: "64Mi"
           cpu: "250m"
         limits:
           memory: "128Mi"
           cpu: "500m"
   ```

2. Apply the configuration:  
   ```bash
   kubectl apply -f resource-limited-pod.yaml
   ```

---

### **70. Task:** Enable and configure audit logs in Kubernetes.

**Answer:**  
1. Set up an audit policy file:  
   ```yaml
   apiVersion: audit.k8s.io/v1
   kind: Policy
   rules:
   - level: Metadata
     verbs: ["create", "delete"]
     resources:
     - group: ""
       resources: ["pods"]
   ```

2. Enable audit logs in `kube-apiserver`:  
   ```bash
   --audit-policy-file=/etc/kubernetes/audit-policy.yaml
   --audit-log-path=/var/log/kubernetes/audit.log
   ```

3. Verify the logs:  
   ```bash
   tail -f /var/log/kubernetes/audit.log
   ```

---

### **71. Task:** Secure Kubernetes API server with TLS authentication.

**Answer:**  
1. Generate a certificate and key for the `kube-apiserver`:
   ```bash
   openssl genpkey -algorithm RSA -out /etc/kubernetes/pki/apiserver.key
   openssl req -new -key /etc/kubernetes/pki/apiserver.key -out /etc/kubernetes/pki/apiserver.csr
   openssl x509 -req -in /etc/kubernetes/pki/apiserver.csr -CA /etc/kubernetes/pki/ca.crt -CAkey /etc/kubernetes/pki/ca.key -CAcreateserial -out /etc/kubernetes/pki/apiserver.crt
   ```

2. Configure `kube-apiserver` to use the TLS certificate:
   ```bash
   --tls-cert-file=/etc/kubernetes/pki/apiserver.crt
   --tls-private-key-file=/etc/kubernetes/pki/apiserver.key
   ```

---

### **72. Task:** Rotate API server certificates in Kubernetes.

**Answer:**  
1. Rotate certificates using `kubeadm`:
   ```bash
   kubeadm certs renew all
   ```

2. Replace the certificates on each node:
   ```bash
   cp /etc/kubernetes/pki/apiserver.crt /etc/kubernetes/pki/apiserver.key /etc/kubernetes/pki/
   ```

3. Restart `kubelet` and other components.

---

### **73. Task:** Set up a VPN between Kubernetes clusters for cross-cluster communication.

**Answer:**  
1. Set up a **VPN Gateway** using OpenVPN or other tools between clusters.
2. Configure **NetworkPolicies** in each cluster to allow traffic from the other cluster's IP range.
3. Deploy a **service** with `ExternalIPs` or use `Ingress` resources to manage cross-cluster traffic securely.

---

### **74. Task:** Isolate namespaces using Network Policies.

**Answer:**  
1. Create a default-deny NetworkPolicy for the namespace `default`:  
   ```yaml
   apiVersion: networking.k8s.io/v1
   kind: NetworkPolicy
   metadata:
     name: default-deny
     namespace: default
   spec:
     podSelector: {}
     policyTypes:
     - Ingress
     - Egress
   ```

2. Allow access only from a specific IP block:
   ```yaml
   ingress:
   - from:
     - ipBlock:
         cidr: 192.168.0.0/16
   ```

---

### **75. Task:** How to prevent privilege escalation in Kubernetes Pods?

**Answer:**  
To prevent privilege escalation in Kubernetes Pods, use the `securityContext` settings in your Pod specs:
1. **Disable privilege escalation** for containers:
   ```yaml
   apiVersion: v1
   kind: Pod
   metadata:
     name: secure-pod
   spec:
     containers:
     - name: nginx
       image: nginx
       securityContext:
         allowPrivilegeEscalation: false
   ```
2. **Use security contexts** to ensure Pods run with non-root privileges.

---

### **76. Task:** Secure Docker images by using trusted base images.

**Answer:**  
1. Start by using official images or images from trusted sources:
   ```Dockerfile
   FROM nginx:latest
   ```
2. Scan the image for vulnerabilities using tools like **Trivy** or **Clair**:
   ```bash
   trivy image nginx:latest
   ```
3. Regularly update images to patch known vulnerabilities.

---

### **77. Task:** Protect Kubernetes secrets in production using encryption.

**Answer:**  
1. Enable encryption at rest for Kubernetes secrets by modifying the `kube-apiserver` configuration with the `encryption-provider-config`:
   ```yaml
   apiVersion: apiserver.config.k8s.io/v1
   kind: EncryptionConfiguration
   resources:
     - resources:
         - secrets
       providers:
         - aescbc:
             keys:
               - name: key1
                 secret: <encrypted-key>
   ```

2. Use `kube-apiserver` flags:
   ```bash
   --encryption-provider-config=/etc/kubernetes/encryption-config.yaml
   ```

3. Verify encryption by checking secrets in etcd.

---

### **78. Task:** Implement audit logging for Kubernetes cluster activities.

**Answer:**  
1. Configure Kubernetes audit logging to track API requests:
   ```yaml
   apiVersion: audit.k8s.io/v1
   kind: Policy
   rules:
   - level: Metadata
     verbs: ["create", "delete", "update"]
     resources:
     - group: ""
       resources: ["pods", "services"]
   ```

2. Set up `kube-apiserver` to log audit data:
   ```bash
   --audit-policy-file=/etc/kubernetes/audit-policy.yaml
   --audit-log-path=/var/log/kubernetes/audit.log
   ```

3. Verify logs:
   ```bash
   tail -f /var/log/kubernetes/audit.log
   ```

---

### **79. Task:** How to secure Kubernetes ingress with HTTPS?

**Answer:**  
1. Create an SSL/TLS certificate using **Let's Encrypt** or any other provider:
   ```bash
   kubectl create secret tls my-tls-secret --cert=cert.crt --key=cert.key -n default
   ```

2. Configure Ingress resource with TLS termination:
   ```yaml
   apiVersion: networking.k8s.io/v1
   kind: Ingress
   metadata:
     name: secure-ingress
     annotations:
       nginx.ingress.kubernetes.io/ssl-redirect: "true"
   spec:
     tls:
     - hosts:
       - myapp.example.com
       secretName: my-tls-secret
     rules:
     - host: myapp.example.com
       http:
         paths:
         - path: /
           pathType: Prefix
           backend:
             service:
               name: myapp-service
               port:
                 number: 80
   ```

---

### **80. Task:** Use network policies to isolate sensitive namespaces in Kubernetes.

**Answer:**  
1. Define a **NetworkPolicy** to restrict traffic between namespaces:
   ```yaml
   apiVersion: networking.k8s.io/v1
   kind: NetworkPolicy
   metadata:
     name: restrict-access
     namespace: sensitive-namespace
   spec:
     podSelector: {}
     policyTypes:
     - Ingress
     - Egress
     ingress:
     - from:
       - namespaceSelector:
           matchLabels:
             name: secure-namespace
   ```

2. This policy allows traffic only from the `secure-namespace` to `sensitive-namespace`.

---

### **81. Task:** How to use Pod Security Policies (PSP) to enforce security controls?

**Answer:**  
1. **Disable privilege escalation** for containers:
   ```yaml
   apiVersion: policy/v1beta1
   kind: PodSecurityPolicy
   metadata:
     name: non-root-policy
   spec:
     runAsUser:
       rule: MustRunAsNonRoot
     fsGroup:
       rule: MustRunAs
   ```
2. Apply this policy to ensure that Pods are not running as root users.

---

### **82. Task:** Protect a Kubernetes cluster from DoS attacks using resource limits.

**Answer:**  
1. Set **resource requests** and **limits** for each container:
   ```yaml
   apiVersion: v1
   kind: Pod
   metadata:
     name: resource-limited-pod
   spec:
     containers:
     - name: app
       image: nginx
       resources:
         requests:
           memory: "64Mi"
           cpu: "250m"
         limits:
           memory: "128Mi"
           cpu: "500m"
   ```
2. Use Horizontal Pod Autoscaling to protect the cluster from overload.

---

### **83. Task:** Configure a Kubernetes service with an external IP.

**Answer:**  
1. Expose the service to external traffic:
   ```yaml
   apiVersion: v1
   kind: Service
   metadata:
     name: my-service
   spec:
     type: LoadBalancer
     selector:
       app: myapp
     ports:
       - port: 80
         targetPort: 8080
   ```

2. The service will expose itself with an external IP.

---

### **84. Task:** How to secure Kubernetes cluster nodes using `kubelet` authentication?

**Answer:**  
1. Use **X.509 certificates** for kubelet authentication.
2. Enable **client authentication** and **authorization** by setting flags in the `kubelet` config:
   ```bash
   --client-ca-file=/etc/kubernetes/pki/ca.crt
   --kubelet-client-certificate=/etc/kubernetes/pki/kubelet-client.crt
   --kubelet-client-key=/etc/kubernetes/pki/kubelet-client.key
   ```

---

### **85. Task:** How to perform a Kubernetes cluster upgrade (using `kubeadm`)?

**Answer:**  
1. Update `kubeadm`:
   ```bash
   apt-get update && apt-get upgrade kubeadm
   ```

2. Upgrade the cluster:
   ```bash
   kubeadm upgrade plan
   kubeadm upgrade apply v1.22.3
   ```

3. Upgrade `kubelet` and `kubectl`:
   ```bash
   apt-get update && apt-get upgrade kubelet kubectl
   ```

---

### **86. Task:** How to configure resource quotas to limit resource consumption in a namespace?

**Answer:**  
1. Define resource quotas for CPU, memory, and Pods in the namespace:
   ```yaml
   apiVersion: v1
   kind: ResourceQuota
   metadata:
     name: cpu-memory-quota
     namespace: my-namespace
   spec:
     hard:
       requests.cpu: "4"
       requests.memory: "8Gi"
       pods: "10"
   ```

2. Apply and validate resource quotas:
   ```bash
   kubectl apply -f resource-quota.yaml
   ```

---

### **87. Task:** Enforce the use of secure images in Kubernetes Pods.

**Answer:**  
1. Create an **ImagePolicy** to enforce the use of images from trusted sources:
   ```yaml
   apiVersion: admission.k8s.io/v1
   kind: MutatingAdmissionWebhook
   metadata:
     name: secure-image-policy
   spec:
     rules:
       - operations: ["CREATE"]
         resources: ["pods"]
         namespaceSelector: {}
   ```

2. Validate the usage of the policy.

---

### **88. Task:** How to detect and mitigate malicious container behaviors?

**Answer:**  
1. Use security tools like **Falco** to monitor container behavior.
2. Implement runtime security tools to detect unexpected behaviors like:
   - Privilege escalation attempts
   - Process anomalies
   - Network activity monitoring

---

### **89. Task:** How to handle failed deployments using ArgoCD?

**Answer:**  
1. Configure ArgoCD to perform automatic rollbacks on deployment failure.
2. Example of setting up **rollout strategy** in ArgoCD:
   ```yaml
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     name: myapp-deployment
   spec:
     replicas: 3
     strategy:
       type: RollingUpdate
       rollingUpdate:
         maxSurge: 1
         maxUnavailable: 0
   ```

3. ArgoCD will automatically monitor the status and revert if an issue arises.

---

### **90. Task:** How to secure secrets in Kubernetes?

**Answer:**  
1. Use **Kubernetes Secrets** and ensure that secrets are encrypted at rest:
   - Enable encryption for Kubernetes secrets in `kube-apiserver`.
   - Example for encryption at rest:
     ```yaml
     apiVersion: apiserver.config.k8s.io/v1
     kind: EncryptionConfiguration
     resources:
       - resources:
           - secrets
         providers:
           - aescbc:
               keys:
                 - name: key1
                   secret: <encrypted-key>
     ```

2. Access secrets using Kubernetes API and mount them into Pods securely.

---

### **91. Task:** How do you protect Kubernetes nodes from unauthorized access?

**Answer:**  
1. **Use kubelet authentication** to ensure that only authorized users can access the node:
   - Set `client-ca-file` and `kubelet-client-certificate` flags in `kubelet`.
   ```bash
   --client-ca-file=/etc/kubernetes/pki/ca.crt
   --kubelet-client-certificate=/etc/kubernetes/pki/kubelet-client.crt
   --kubelet-client-key=/etc/kubernetes/pki/kubelet-client.key
   ```

2. Enable **RBAC (Role-Based Access Control)** to limit permissions.

3. Secure **SSH access** to nodes and ensure only specific users or teams can access the node.

---

### **92. Task:** How to secure Kubernetes API server?

**Answer:**  
1. **Enable audit logging** to monitor access and actions:
   ```bash
   --audit-policy-file=/etc/kubernetes/audit-policy.yaml
   --audit-log-path=/var/log/kubernetes/audit.log
   ```

2. Enforce **RBAC policies** to control who can access the API and limit privileges:
   - Example of restricting access to Kubernetes API:
     ```yaml
     apiVersion: rbac.authorization.k8s.io/v1
     kind: Role
     metadata:
       namespace: default
       name: api-read-only
     rules:
     - apiGroups: [""]
       resources: ["pods"]
       verbs: ["get", "list"]
     ```

3. Use **API server flags** to disable unauthenticated access:
   ```bash
   --anonymous-auth=false
   ```

---

### **93. Task:** How to prevent containers from running as root in Kubernetes?

**Answer:**  
1. Use **Pod Security Policies (PSP)** to enforce non-root containers:
   ```yaml
   apiVersion: policy/v1beta1
   kind: PodSecurityPolicy
   metadata:
     name: non-root-policy
   spec:
     runAsUser:
       rule: MustRunAsNonRoot
   ```

2. Add the `securityContext` in your Pod configuration:
   ```yaml
   apiVersion: v1
   kind: Pod
   metadata:
     name: non-root-pod
   spec:
     containers:
     - name: app
       image: nginx
       securityContext:
         runAsUser: 1000
         runAsGroup: 3000
   ```

---

### **94. Task:** How to secure a Kubernetes network using NetworkPolicies?

**Answer:**  
1. Define a **NetworkPolicy** to restrict traffic between Pods:
   ```yaml
   apiVersion: networking.k8s.io/v1
   kind: NetworkPolicy
   metadata:
     name: allow-pod-to-pod-communication
     namespace: default
   spec:
     podSelector: {}
     policyTypes:
     - Ingress
     ingress:
     - from:
       - podSelector:
           matchLabels:
             app: frontend
   ```

2. The above policy restricts access to Pods from other namespaces and allows only specific communication.

---

### **95. Task:** How do you ensure the security of your Kubernetes container images?

**Answer:**  
1. Use **trusted base images** and scan images for vulnerabilities:
   - Use **Trivy** to scan images:
     ```bash
     trivy image my-app:latest
     ```

2. Implement **image signing** to verify the integrity of your container images:
   - Use **Notary** to sign images.

3. Regularly update and patch base images to reduce known vulnerabilities.

---

### **96. Task:** How to protect Kubernetes clusters against **DoS (Denial of Service)** attacks?

**Answer:**  
1. Implement **resource limits** for all Pods and containers:
   ```yaml
   apiVersion: v1
   kind: Pod
   metadata:
     name: secure-pod
   spec:
     containers:
     - name: nginx
       image: nginx
       resources:
         requests:
           memory: "64Mi"
           cpu: "250m"
         limits:
           memory: "128Mi"
           cpu: "500m"
   ```

2. **Enable Horizontal Pod Autoscaler (HPA)** to scale based on load:
   ```yaml
   apiVersion: autoscaling/v2beta2
   kind: HorizontalPodAutoscaler
   metadata:
     name: hpa-example
     namespace: default
   spec:
     scaleTargetRef:
       apiVersion: apps/v1
       kind: Deployment
       name: my-app
     minReplicas: 2
     maxReplicas: 10
     metrics:
       - type: Resource
         resource:
           name: cpu
           target:
             type: Utilization
             averageUtilization: 80
   ```

---

### **97. Task:** How to use Helm to manage Kubernetes configurations securely?

**Answer:**  
1. **Install Helm** and initialize the Helm environment:
   ```bash
   helm init
   ```

2. Use **Helm secrets** to store sensitive data:
   - Use `helm-secrets` plugin to encrypt and decrypt secrets in Helm charts.

3. Create and manage **Helm charts** securely by following RBAC and least privilege principles.

---

### **98. Task:** How to secure Kubernetes with **PodSecurityPolicy**?

**Answer:**  
1. Enable **PodSecurityPolicy (PSP)** on the Kubernetes cluster:
   - Configure PSP with the necessary security restrictions, such as disallowing privilege escalation.
2. Example configuration:
   ```yaml
   apiVersion: policy/v1beta1
   kind: PodSecurityPolicy
   metadata:
     name: restricted-policy
   spec:
     privileged: false
     runAsUser:
       rule: MustRunAsNonRoot
     allowPrivilegeEscalation: false
   ```

---

### **99. Task:** How to audit Kubernetes cluster actions and configure alerts for suspicious activities?

**Answer:**  
1. Set up **audit logging** to track API calls made to the Kubernetes API server:
   ```bash
   --audit-policy-file=/etc/kubernetes/audit-policy.yaml
   --audit-log-path=/var/log/kubernetes/audit.log
   ```

2. Integrate with **SIEM (Security Information and Event Management)** systems like **Splunk** or **Elasticsearch** to monitor suspicious activities.

3. Configure alerts on abnormal events such as privilege escalation, failed authentication attempts, etc.

---

### **100. Task:** How to secure Kubernetes secrets using **Vault**?

**Answer:**  
1. Use **HashiCorp Vault** to manage secrets securely:
   - Store secrets in Vault and use Kubernetes authentication to access them:
     ```bash
     vault auth enable kubernetes
     vault write auth/kubernetes/config \
         kubernetes_host=https://kubernetes.default.svc.cluster.local \
         kubernetes_ca_cert=@/var/run/secrets/kubernetes.io/serviceaccount/ca.crt \
         token_reviewer_jwt=@/var/run/secrets/kubernetes.io/serviceaccount/token
     ```

2. Mount the secrets directly into your Pods using Kubernetes `secrets` API.

---
You're right to ask that! The **Certified Kubernetes Security Specialist (CKS)** exam covers a wide array of topics related to Kubernetes security, including **cluster security**, **container security**, **network security**, **storage security**, **CI/CD pipeline security**, **logging, monitoring, and auditing**, and more.

Let me break down the **CKS exam objectives** and ensure that **all key topics** are properly covered:

---

### **CKS Exam Topics**:

1. **Cluster Hardening**:  
   - Securing the Kubernetes API server.
   - Securing nodes and clusters.
   - Using PodSecurityPolicy (PSP).
   - Restricting access with RBAC.
   - Hardening etcd.

2. **Workload Security**:  
   - Securing containers and images.
   - Using container runtime security best practices.
   - Implementing **Kubernetes NetworkPolicies**.
   - Securing multi-tenancy and namespaces.
   - Securing Kubernetes workloads like Pods, Deployments, DaemonSets, StatefulSets.
  
3. **Supply Chain Security**:  
   - Managing secure container images.
   - Image scanning (e.g., Trivy, Clair).
   - Signing container images.
   - Preventing container privilege escalation.
   
4. **CI/CD Pipeline Security**:  
   - Integrating Kubernetes with CI/CD tools securely.
   - Secure application deployment practices.
   - Integrating Kubernetes with **Helm** securely.
   
5. **Logging, Monitoring, and Auditing**:  
   - Setting up and securing logging.
   - Monitoring cluster activity (Prometheus, Grafana).
   - Integrating audit logging for security events.
   - Centralized logging and SIEM systems.
   
6. **Network Security**:  
   - Securing Kubernetes networking.
   - Network policies for controlling communication.
   - Securing traffic with **TLS encryption**.
   - Use of **Istio** for secure service-to-service communication.
   - Securing ingress and egress traffic.

7. **Secret Management**:  
   - Managing Kubernetes secrets securely.
   - Integrating Kubernetes with **HashiCorp Vault**.
   - Encrypting secrets at rest and in transit.
   
8. **Cluster Observability**:  
   - Integrating observability tools (Prometheus, Grafana).
   - Jaeger for tracing.
   - Monitoring cluster security events.

---

### **101. Task:** How do you implement security in Kubernetes CI/CD pipelines?

**Answer:**
1. Use **RBAC** to limit access to CI/CD resources and deployments.
2. Implement **image scanning** for vulnerabilities before deployment (use tools like **Trivy** or **Clair**).
3. Use **Helm** with the `--verify` flag to ensure Helm charts are from trusted sources.

---

### **102. Task:** How to enable Kubernetes **Audit Logs** and why is it important?

**Answer:**
1. Configure **audit logs** on the API server:
   ```bash
   --audit-policy-file=/etc/kubernetes/audit-policy.yaml
   --audit-log-path=/var/log/kubernetes/audit.log
   ```
2. Audit logs help detect unauthorized access, privilege escalation, and policy violations.
3. Ensure proper filtering and retention to maintain a secure logging environment.

---

### **103. Task:** How do you securely manage secrets in Kubernetes?

**Answer:**
1. Use **Kubernetes Secrets** to store sensitive information but ensure they are encrypted at rest using **EncryptionConfiguration** in the API server.
2. Integrate with **HashiCorp Vault** for managing secrets centrally.
3. Use **sealed secrets** (via **kubeseal**) for encrypted secrets.

---

### **104. Task:** What is **PodSecurityPolicy (PSP)** and how does it help in securing Pods?

**Answer:**
1. **PSP** allows defining security conditions for Pods:
   - Preventing privilege escalation.
   - Ensuring only non-root containers can run.
   - Restricting which users can execute certain operations inside the container.
2. Example:
   ```yaml
   apiVersion: policy/v1beta1
   kind: PodSecurityPolicy
   metadata:
     name: restricted-policy
   spec:
     privileged: false
     runAsUser:
       rule: MustRunAsNonRoot
     allowPrivilegeEscalation: false
   ```

---

### **105. Task:** How do you enforce the **least privilege** for Kubernetes services?

**Answer:**
1. Use **RBAC** to grant only the minimum required permissions.
2. Use service accounts with limited scopes and **namespaced** roles.
3. Example:
   ```yaml
   apiVersion: rbac.authorization.k8s.io/v1
   kind: Role
   metadata:
     namespace: default
     name: app-role
   rules:
     - apiGroups: [""]
       resources: ["pods"]
       verbs: ["get", "list"]
   ```

---

### **106. Task:** How do you secure Kubernetes ingress resources?

**Answer:**
1. Use **TLS termination** at the ingress controller level.
2. Example:
   ```yaml
   apiVersion: networking.k8s.io/v1
   kind: Ingress
   metadata:
     name: my-ingress
   spec:
     tls:
       - hosts:
           - "example.com"
         secretName: tls-secret
     rules:
       - host: "example.com"
         http:
           paths:
             - path: /
               pathType: Prefix
               backend:
                 service:
                   name: my-service
                   port:
                     number: 80
   ```

---

### **107. Task:** How to perform a **Kubernetes security audit** on the running cluster?

**Answer:**
1. Use tools like **Kube-bench** or **kubesec.io** to run security checks against your cluster.
2. Review **audit logs** for any suspicious activity.
3. Regularly scan **container images** and ensure they are free of vulnerabilities.

---

### **108. Task:** How do you prevent unauthorized access to Kubernetes API?

**Answer:**
1. Disable **anonymous authentication** by setting:
   ```bash
   --anonymous-auth=false
   ```
2. Use **RBAC** to enforce strict access controls and ensure only authorized users can interact with the API server.
3. Enable **API server audit logging** to track unauthorized access attempts.

---

### **109. Task:** What are the best practices for securing Kubernetes **networking**?

**Answer:**
1. Use **NetworkPolicies** to control pod-to-pod communication.
2. Implement **service meshes** (e.g., Istio) for encryption of inter-service communication.
3. Ensure **DNS resolution** inside the cluster is protected and only allowed pods can resolve services.

---

### **110. Task:** How do you handle **container vulnerabilities** in a Kubernetes environment?

**Answer:**
1. Use **container image scanning** tools like **Trivy**, **Clair**, or **Anchore**.
2. Implement **signed images** and verify the integrity of images before deploying them.
3. Ensure containers are **running with minimal privileges** using security contexts in the Pod specification.

---

### **111. Task:** How do you secure Kubernetes with **Role-Based Access Control (RBAC)**?

**Answer:**
1. Define **Roles** and **RoleBindings** to ensure that only authorized users have the necessary permissions.
2. Use **ClusterRoles** for cluster-wide access control and **RoleBindings** for namespace-scoped access.
3. Example of creating a read-only role:
   ```yaml
   apiVersion: rbac.authorization.k8s.io/v1
   kind: Role
   metadata:
     namespace: default
     name: read-only
   rules:
     - apiGroups: [""]
       resources: ["pods"]
       verbs: ["get", "list"]
   ```

---

### **112. Task:** How can you protect Kubernetes nodes from external attacks?

**Answer:**
1. Disable unnecessary ports and services on nodes.
2. Use **firewalls** to limit inbound traffic to the nodes.
3. Ensure **kubelet** is not exposed publicly by setting `--read-only-port=0`.
4. Use **Kubernetes NetworkPolicies** to block unauthorized access from external sources.

---

### **113. Task:** How do you manage **Kubernetes Secrets** securely?

**Answer:**
1. **Encrypt** Kubernetes secrets at rest using `EncryptionConfiguration` in `kube-apiserver`.
2. Use **Vault** to manage secrets and integrate with Kubernetes.
3. Rotate secrets regularly and use **Kubernetes RBAC** to control access to secrets.

---

### **114. Task:** What is **PodSecurityPolicy (PSP)** and how do you use it?

**Answer:**
1. **PSP** defines security requirements for Pods like restricting privileged containers, ensuring non-root users, and limiting access to sensitive APIs.
2. Example:
   ```yaml
   apiVersion: policy/v1beta1
   kind: PodSecurityPolicy
   metadata:
     name: restricted-policy
   spec:
     privileged: false
     runAsUser:
       rule: MustRunAsNonRoot
   ```

---

### **115. Task:** How do you secure **Kubernetes Cluster Components**?

**Answer:**
1. Secure the **Kubelet** by disabling unauthenticated access and configuring client certificate authentication.
2. Harden **etcd** by enabling encryption and restricting access to it via firewalls or network segmentation.
3. Implement **Role-Based Access

 Control (RBAC)** for accessing the Kubernetes API server and controlling cluster resources.

---

### **116. How do you secure a Kubernetes cluster from unauthorized access?**
**Answer**:
- Use **RBAC** (Role-Based Access Control) to restrict access to Kubernetes resources.
- Enable **API server authentication** using certificates or tokens.
- Use **Network Policies** to control traffic between Pods.
- Enable **audit logging** for monitoring access.

---

### **117. What is the difference between a Kubernetes Role and ClusterRole?**
**Answer**:
- **Role**: Provides permissions within a specific namespace.
- **ClusterRole**: Provides permissions across the entire cluster.
- **Use Case**: ClusterRoles are often used for setting up permissions for cluster-wide resources like nodes or storage classes.

---

### **118. How do you secure sensitive data in Kubernetes?**
**Answer**:
- Use **Kubernetes Secrets** for storing sensitive data such as passwords or tokens.
- Encrypt secrets at rest using **KMS** (Key Management Service).
- Use **RBAC** and **Network Policies** to restrict access to Secrets.

---

### **119. How would you limit container privileges in Kubernetes?**
**Answer**:
- Set **Security Context** in Pods to limit privileges.
- Use **PodSecurityPolicies (PSPs)** or **OPA (Open Policy Agent)** to enforce security standards.
- Avoid running containers as root; instead, define non-root user IDs in the container spec.

---

### **120. What is the use of **Admission Controllers** in Kubernetes?**
**Answer**:
- **Admission Controllers** enforce security policies and actions on requests made to the Kubernetes API server.
- Examples include **PodSecurityPolicy**, **MutatingAdmissionWebhook**, and **ValidatingAdmissionWebhook**.

---

### **121. How do you prevent privilege escalation in Kubernetes?**
**Answer**:
- Define **non-root users** for containers.
- Use **Security Contexts** to prevent escalations to root.
- Implement **PodSecurityPolicies (PSP)** to enforce security constraints.

---

### **122. How can you monitor Kubernetes resource usage in real-time?**
**Answer**:
- Use **Prometheus** for monitoring metrics such as CPU, memory usage, and pod health.
- Set up **Grafana dashboards** to visualize metrics and set up alerts.

---

### **123. What is **NetworkPolicy** in Kubernetes and how does it work?**
**Answer**:
- **NetworkPolicy** defines the allowed traffic flow between Pods.
- It can be used to block traffic to and from specific Pods, restricting communication between namespaces.

**Example**:
```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-web
spec:
  podSelector:
    matchLabels:
      role: web
  ingress:
    - from:
      - podSelector:
          matchLabels:
            role: frontend
```

---

### **124. What is the importance of **PodSecurityPolicies (PSP)** in securing Kubernetes clusters?**
**Answer**:
- **PodSecurityPolicies** define a set of conditions for pod deployment (e.g., restricting privilege escalation, restricting the use of host namespaces).
- PSPs can be used to enforce compliance with security requirements across the cluster.

---

### **125. How would you ensure Kubernetes secrets are encrypted?**
**Answer**:
- Enable **encryption at rest** for Secrets using **KMS (Key Management Service)**.
- Configure the **API Server** with encryption providers to encrypt sensitive data stored in etcd.

---

### **126. How do you configure **PodSecurityPolicy** to ensure containers do not run as root?**
**Answer**:
```yaml
apiVersion: policy/v1beta1
kind: PodSecurityPolicy
metadata:
  name: restrict-root-user
spec:
  runAsUser:
    rule: 'MustRunAsNonRoot'
```
This ensures that all containers must run as non-root users.

---

### **127. How do you enforce image security in Kubernetes?**
**Answer**:
- Use **ImagePolicyWebhook** to enforce security policies around the use of Docker images.
- Set up **Kubernetes ImagePullSecrets** to ensure only trusted sources are used for pulling container images.
- Scan images for vulnerabilities using tools like **Trivy** and **Clair**.

---

### **128. How would you perform a security audit of a Kubernetes cluster?**
**Answer**:
- Enable **audit logging** for API access and actions.
- Use tools like **KubeAudit** or **kube-score** to check for misconfigurations.
- Review access control settings, network policies, and resource limits for potential vulnerabilities.

---

### **129. How do you implement a secure **Ingress Controller** in Kubernetes?**
**Answer**:
- Use **NGINX** or **Traefik** Ingress controllers to securely expose services.
- Enforce TLS termination with certificates stored in Kubernetes secrets.
- Use **NetworkPolicies** to restrict access to the Ingress controller.

---

### **130. How would you handle logging in a Kubernetes environment?**
**Answer**:
- Use **Fluentd** or **EFK stack** (Elasticsearch, Fluentd, Kibana) to aggregate and process logs.
- Store logs in a central location to make troubleshooting easier and to monitor for suspicious activity.
  
---

### **131. Explain **Audit Logs** in Kubernetes. How would you configure them?**
**Answer**:
- **Audit Logs** track all interactions with the Kubernetes API server.
- Configure **audit policy** in the API server by specifying which events to capture, including `metadata` like user identity, timestamp, and the action taken.

---

### **132. What are **Admission Controllers** and how do they improve security?**
**Answer**:
- Admission Controllers enforce policy decisions during the Kubernetes API request process.
- Example: **ValidatingAdmissionWebhook** can block deployment of certain containers or images that do not meet security criteria.

---

### **133. What are some of the best practices for securing Kubernetes API Server?**
**Answer**:
- Enforce **RBAC** for all users and services.
- Disable **anonymous access** and use **client certificates** for authentication.
- Enable **audit logging** and restrict access to sensitive resources.

---

### **134. How would you use **RBAC (Role-Based Access Control)** in Kubernetes for controlling user access?**
**Answer**:
- Define **Roles** and **ClusterRoles** for specific permissions.
- Bind Roles to users via **RoleBindings** and **ClusterRoleBindings**.
- Example:
```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: default
  name: pod-reader
rules:
  - apiGroups: [""]
    resources: ["pods"]
    verbs: ["get", "list"]
```

---

### **135. How can you prevent unauthorized access to sensitive Kubernetes resources?**
**Answer**:
- Use **RBAC** to grant access only to necessary resources.
- Apply **Network Policies** to restrict network traffic.
- Use **Kubernetes Secrets** and encrypt them at rest.

---

### **136. What is the purpose of **Kubernetes Network Policies**, and how are they used?**
**Answer**:
- **Network Policies** define how Pods communicate with each other within the cluster.
- It restricts traffic between Pods based on labels and namespaces.

**Example**:
```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-pod-communication
spec:
  podSelector:
    matchLabels:
      app: frontend
  ingress:
  - from:
    - podSelector:
        matchLabels:
          app: backend
```

---

### **137. How can you secure Kubernetes container runtimes?**
**Answer**:
- Use **gVisor** or **Kata Containers** for enhanced security.
- Configure runtime security settings such as restricting container capabilities and namespaces.

---

### **138. What is **ServiceAccount** in Kubernetes and how does it enhance security?**
**Answer**:
- **ServiceAccount** provides an identity for processes running in Pods to interact with the Kubernetes API.
- It enables access control through RBAC and limits permissions based on the service’s needs.

---

### **139. How do you limit resource usage (CPU/Memory) in Kubernetes?**
**Answer**:
- Use **Resource Requests and Limits** in the Pod specification to ensure containers have the appropriate amount of resources.

**Example**:
```yaml
resources:
  requests:
    memory: "64Mi"
    cpu: "250m"
  limits:
    memory: "128Mi"
    cpu: "500m"
```

---

### **140. What are the key components of Kubernetes security architecture?**
**Answer**:
- **API Server**: Central management of the Kubernetes control plane.
- **Kubelet**: Ensures the correct state of Pods.
- **RBAC**: Role-based permissions management.
- **Network Policies**: Controls the communication between Pods.
- **Secrets Management**: Securely stores sensitive data.

---
