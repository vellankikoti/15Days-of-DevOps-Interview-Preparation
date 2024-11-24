### **Kubernetes - 100 Questions**

---

**1. What is Kubernetes?**
- **Answer**: Kubernetes is an open-source container orchestration platform for automating the deployment, scaling, and management of containerized applications.

---

**2. What is a Pod in Kubernetes?**
- **Answer**: A Pod is the smallest deployable unit in Kubernetes. It consists of one or more containers that share storage, networking, and a specification for how to run the containers.

---

**3. What is the difference between a Pod and a Container?**
- **Answer**: A Pod can contain one or more containers that share the same network and storage. A container is a lightweight, standalone package that holds an application and its dependencies.

---

**4. What is a ReplicaSet in Kubernetes?**
- **Answer**: A ReplicaSet ensures that a specified number of pod replicas are running at any given time. If a pod crashes or is deleted, the ReplicaSet will automatically create a new pod to maintain the desired state.

---

**5. How does a Deployment differ from a ReplicaSet?**
- **Answer**: A Deployment manages ReplicaSets and provides declarative updates to applications. It ensures that the right number of replicas are running, and handles rolling updates and rollbacks.

---

**6. What is Kubernetes Service?**
- **Answer**: A Service is an abstraction that defines a logical set of Pods and a policy to access them, typically through a load balancer or other network proxies.

---

**7. What are the different types of Services in Kubernetes?**
- **Answer**: The four main types of Services in Kubernetes are:
  - **ClusterIP**: Exposes the service on a cluster-internal IP.
  - **NodePort**: Exposes the service on each Node’s IP at a static port.
  - **LoadBalancer**: Exposes the service externally using a cloud provider’s load balancer.
  - **ExternalName**: Maps the service to an external DNS name.

---

**8. What is Kubernetes Ingress?**
- **Answer**: An Ingress is an API object that manages external access to services in a cluster, typically HTTP, and allows you to define routing rules based on hostnames or paths.

---

**9. How do you create a Kubernetes Deployment?**
- **Answer**: A Kubernetes Deployment can be created using `kubectl apply -f deployment.yaml` or `kubectl create deployment my-app --image=my-image`.

---

**10. What is a ConfigMap in Kubernetes?**
- **Answer**: A ConfigMap is a Kubernetes API object used to store non-confidential configuration data in key-value pairs, which can be used by Pods at runtime.

---

**11. What is a Secret in Kubernetes?**
- **Answer**: A Secret is used to store sensitive data, such as passwords, OAuth tokens, SSH keys, etc. It is encoded and can be used by Pods securely.

---

**12. What is Kubernetes Namespace?**
- **Answer**: A namespace is a way to divide cluster resources between multiple users or applications. It allows you to group resources together and separate them logically.

---

**13. What is Horizontal Pod Autoscaling (HPA)?**
- **Answer**: HPA automatically adjusts the number of pod replicas in a deployment or replica set based on observed CPU utilization or other select metrics.

---

**14. How do you scale a Deployment in Kubernetes?**
- **Answer**: You can scale a deployment using `kubectl scale deployment my-deployment --replicas=3`.

---

**15. What is Vertical Pod Autoscaling (VPA)?**
- **Answer**: VPA automatically adjusts the CPU and memory requests and limits of pods based on usage metrics, which helps optimize resource allocation.

---

**16. How do you upgrade a Kubernetes cluster?**
- **Answer**: To upgrade a Kubernetes cluster, use `kubeadm upgrade` for on-prem clusters or leverage cloud provider tools for managed services like EKS or GKE.

---

**17. What is the Kubernetes Control Plane?**
- **Answer**: The Kubernetes Control Plane is the brain of the Kubernetes cluster. It manages the cluster’s state, schedules workloads, and ensures the desired state is met.

---

**18. What is the role of kubelet in Kubernetes?**
- **Answer**: The kubelet is an agent running on each node in the cluster. It ensures the containers in the pods are running as expected.

---

**19. What is a DaemonSet in Kubernetes?**
- **Answer**: A DaemonSet ensures that a copy of a pod runs on all or selected nodes in a cluster. It is often used for logging, monitoring, or other node-level tasks.

---

**20. What is the role of Kubernetes Scheduler?**
- **Answer**: The Kubernetes Scheduler is responsible for assigning pods to available nodes based on resource availability, affinity rules, taints, and tolerations.

---

**21. How does Kubernetes handle stateful applications?**
- **Answer**: Kubernetes handles stateful applications using StatefulSets, which provide unique network identities and stable storage for each pod.

---

**22. What is a Persistent Volume (PV)?**
- **Answer**: A Persistent Volume is a piece of storage in the Kubernetes cluster that has been provisioned by an administrator or dynamically created using Storage Classes.

---

**23. What is a Persistent Volume Claim (PVC)?**
- **Answer**: A Persistent Volume Claim is a request for storage by a user. It specifies the size, access modes, and other characteristics needed for the volume.

---

**24. What is the difference between StatefulSet and Deployment?**
- **Answer**: A StatefulSet is for managing stateful applications that require stable storage and persistent identities, whereas a Deployment is for stateless applications.

---

**25. What are Taints and Tolerations in Kubernetes?**
- **Answer**: Taints allow nodes to repel certain pods, and tolerations allow pods to be scheduled on nodes with matching taints.

---

**26. What is a Job in Kubernetes?**
- **Answer**: A Job creates one or more pods and ensures that a specified number of them successfully terminate. It is used for batch processing.

---

**27. How do you configure network policies in Kubernetes?**
- **Answer**: Network Policies control the traffic between Pods. You can define them using YAML files and apply them using `kubectl apply -f network-policy.yaml`.

---

**28. What is Istio and how does it integrate with Kubernetes?**
- **Answer**: Istio is a service mesh that provides advanced networking, observability, and security features for microservices deployed in Kubernetes.

---

**29. What is a Kubernetes Ingress Controller?**
- **Answer**: An Ingress Controller is a load balancer for Kubernetes that listens to Ingress resources and routes HTTP/HTTPS traffic to appropriate services.

---

**30. What are the common troubleshooting steps when a Pod is not starting?**
- **Answer**: Check the pod status using `kubectl describe pod <pod-name>`, view the logs with `kubectl logs`, and ensure that required resources like CPU and memory are available.

---

**31. How do you monitor Kubernetes clusters?**
- **Answer**: Kubernetes can be monitored using tools like Prometheus, Grafana, and Elastic Stack. These tools collect metrics, visualize performance, and alert on issues.

---

**32. What are Kubernetes labels and annotations?**
- **Answer**: Labels are used to organize and select subsets of objects, while annotations are used to store non-identifying metadata on objects.

---

**33. How do you expose a Kubernetes service externally?**
- **Answer**: You can expose a service externally using a LoadBalancer type service or by setting up an Ingress resource.

---

**34. What is Helm in Kubernetes?**
- **Answer**: Helm is a package manager for Kubernetes, which helps to define, install, and upgrade Kubernetes applications via charts.

---

**35. What are Kubernetes probes (Liveness, Readiness)?**
- **Answer**: Liveness probes check if a container is running, while readiness probes determine if a container is ready to accept traffic.

---

**36. How do you configure secrets in Kubernetes?**
- **Answer**: Secrets in Kubernetes are created using `kubectl create secret` and can be used by Pods to store sensitive information.

---

**37. How do you debug a pod in Kubernetes?**
- **Answer**: Use `kubectl logs <pod-name>`, `kubectl describe pod <pod-name>`, and `kubectl exec -it <pod-name> -- /bin/bash` to gather debug information.

---

**38. How do you perform a rolling update in Kubernetes?**
- **Answer**: You can perform a rolling update by updating the deployment's container image, and Kubernetes will manage the transition automatically.

---

**39. How do you perform a rollback in Kubernetes?**
- **Answer**: You can rollback a Kubernetes deployment using `kubectl rollout undo deployment <deployment-name>`.

---

**40. What is a Kubernetes scheduler?**
- **Answer**: The Kubernetes scheduler is responsible for assigning Pods to nodes based on available resources, constraints, and policies.

---

**41. What is the difference between a Pod and a ReplicaSet in Kubernetes?**
- **Answer**: A Pod is a single instance of a container, while a ReplicaSet ensures that a specified number of replicas of a pod are running at any time.

---

**42. How does Kubernetes handle autoscaling?**
- **Answer**: Kubernetes supports Horizontal Pod Autoscaling (HPA), which scales the number of pods in a deployment based on metrics such as CPU or memory usage.

---

**43. What are taints and tolerations in Kubernetes?**
- **Answer**: Taints are applied to nodes to repel certain pods, and tolerations are applied to pods to allow them to run on nodes with matching taints.

---

**44. How do you create a Persistent Volume (PV) and Persistent Volume Claim (PVC) in Kubernetes?**
- **Answer**: PVs are created by admins, and PVCs are requests by users for storage. PVCs are linked to PVs based on matching access modes and size.

---

**45. What is the purpose of Kubernetes namespaces?**
- **Answer**: Namespaces allow you to partition the Kubernetes resources, isolating groups of resources within a cluster, typically used to separate environments like dev, test, and prod.

---

**46. How does Kubernetes handle resource requests and limits for containers?**
- **Answer**: Kubernetes allows you to define resource requests (minimum required) and limits (maximum allowed) for CPU and memory for each container to ensure fair resource allocation.

---

**47. What is a StatefulSet in Kubernetes and how does it differ from a Deployment?**
- **Answer**: A StatefulSet is used for stateful applications, ensuring stable network identities and persistent storage. A Deployment is for stateless applications, where pods can be replaced easily.

---

**48. How do you deploy a Helm chart in Kubernetes?**
- **Answer**: To deploy a Helm chart, first install Helm, then use `helm install <release-name> <chart-name>`.

---

**49. What is the role of the kube-apiserver in Kubernetes?**
- **Answer**: The kube-apiserver is the central component of the Kubernetes control plane, exposing the Kubernetes API and handling communication between various components.

---

**50. How does Kubernetes handle pod scheduling?**
- **Answer**: The Kubernetes scheduler places pods onto nodes based on resource requirements, affinity rules, taints, tolerations, and other policies.

---

**51. How do you manage secrets in Kubernetes?**
- **Answer**: Kubernetes allows secrets to be stored in the etcd cluster and can be accessed by Pods securely. Secrets can be created using `kubectl create secret`.

---

**52. How does Kubernetes handle rolling updates and rollbacks?**
- **Answer**: Kubernetes performs rolling updates by updating Pods incrementally while ensuring that the desired number of replicas is maintained. You can rollback by using `kubectl rollout undo`.

---

**53. What are Kubernetes labels and selectors?**
- **Answer**: Labels are key-value pairs associated with Kubernetes objects, and selectors are used to filter resources based on labels.

---

**54. How does Kubernetes perform pod health checks?**
- **Answer**: Kubernetes uses Liveness probes to check if a pod is running and Readiness probes to check if a pod is ready to serve traffic.

---

**55. How do you manage Kubernetes networking?**
- **Answer**: Kubernetes networking is handled through services, network policies, and ingress controllers. Pod communication is managed by a flat network with no NAT between pods in different nodes.

---

**56. What is a network policy in Kubernetes?**
- **Answer**: A network policy in Kubernetes allows you to control the communication between pods. It specifies which pods can communicate with others based on labels, namespaces, and ports.

---

**57. What is Istio, and how does it integrate with Kubernetes?**
- **Answer**: Istio is a service mesh that provides advanced features like traffic management, security, and observability for microservices deployed in Kubernetes.

---

**58. What is a DaemonSet in Kubernetes?**
- **Answer**: A DaemonSet ensures that a specific pod runs on every node in the cluster. It is useful for services like logging, monitoring, or network proxies.

---

**59. How do you manage logging in Kubernetes?**
- **Answer**: Kubernetes integrates with tools like Fluentd, Prometheus, and Grafana for logging, monitoring, and alerting.

---

**60. What is the role of kube-controller-manager?**
- **Answer**: The kube-controller-manager is responsible for running controller processes, which ensure that the desired state of the cluster is maintained.

---

**61. What is a Kubernetes Admission Controller?**
- **Answer**: Admission Controllers are plugins that govern and enforce how the Kubernetes API server handles requests. They allow dynamic admission control for objects during creation.

---

**62. What are Pod Disruption Budgets (PDBs)?**
- **Answer**: A PDB allows you to limit the number of concurrently disrupted pods during voluntary disruptions (like upgrades or node maintenance) to ensure service availability.

---

**63. What is the role of etcd in Kubernetes?**
- **Answer**: etcd is a distributed key-value store that stores all the configuration data and state information of the Kubernetes cluster.

---

**64. How do you upgrade Kubernetes components like kubelet and kubectl?**
- **Answer**: Use `kubeadm` for upgrading control-plane components, and `apt-get` or `yum` for upgrading kubelet and kubectl on worker nodes.

---

**65. What is a ReplicaSet, and how does it ensure high availability in Kubernetes?**
- **Answer**: A ReplicaSet ensures that a specified number of replicas of a pod are running. If a pod fails, the ReplicaSet will create a new one to replace it.

---

**66. How does Kubernetes handle storage and persistent data?**
- **Answer**: Kubernetes uses Persistent Volumes (PV) and Persistent Volume Claims (PVC) to manage and provision storage for pods that require persistent data.

---

**67. How does Kubernetes handle the concept of affinity and anti-affinity?**
- **Answer**: Affinity and anti-affinity rules allow you to control how pods are scheduled relative to other pods based on labels, to optimize placement for performance or fault tolerance.

---

**68. What is the Kubernetes Ingress Controller, and how does it work?**
- **Answer**: An Ingress Controller is a component that manages external access to services in a Kubernetes cluster, typically HTTP. It handles traffic routing based on Ingress resources.

---

**69. How do you create a custom Kubernetes Resource (CRD)?**
- **Answer**: A Custom Resource Definition (CRD) allows you to extend Kubernetes with custom resources. It is created by defining a custom API object in YAML format and applying it using `kubectl`.

---

**70. How do you check the health of a Kubernetes node?**
- **Answer**: You can check the health of a Kubernetes node using `kubectl get nodes` or `kubectl describe node <node-name>` to get detailed information about the node's status.

---

**71. How do you deploy a multi-container pod in Kubernetes?**
- **Answer**: A multi-container pod is created by specifying multiple containers in the `containers` array of the pod spec in the deployment or pod YAML file.

---

**72. What is a Kubernetes Namespace, and why is it used?**
- **Answer**: A Namespace is a way to divide resources within a cluster, useful for isolating environments or teams, such as production, staging, and development.

---

**73. How do you troubleshoot a Kubernetes pod that is stuck in a CrashLoopBackOff state?**
- **Answer**: You can check the pod’s logs using `kubectl logs <pod-name>`. You should also describe the pod using `kubectl describe pod <pod-name>` to check for events and resource constraints.

---

**74. What are Kubernetes controllers, and how do they work?**
- **Answer**: Controllers are control loops in Kubernetes that watch the state of the cluster and make changes as needed to maintain the desired state. Examples include ReplicaSet and Deployment controllers.

---

**75. What is the significance of Kubernetes DNS?**
- **Answer**: Kubernetes DNS provides name resolution for services and pods within the cluster. Services are automatically assigned DNS names, and pods can access them via these names.

---

**76. What is the role of the Kubernetes API Server?**
- **Answer**: The API Server serves as the entry point for all commands and interactions with the cluster. It processes REST API calls, validates them, and updates the cluster state in etcd.

---

**77. How do you use kubectl port-forwarding to access a pod’s application?**
- **Answer**: Use the command `kubectl port-forward pod/<pod-name> <local-port>:<pod-port>` to forward a port from your local machine to a pod in the cluster.

---

**78. How do you scale a stateful application in Kubernetes?**
- **Answer**: Stateful applications should be scaled carefully since they rely on stable identities and persistent storage. Scaling is done via StatefulSets, but you need to ensure that the storage and network are correctly configured.

---

**79. How do you ensure security in Kubernetes?**
- **Answer**: Kubernetes security includes using Role-Based Access Control (RBAC), Network Policies, Secrets management, and following best practices like restricting privileges, using namespaces for isolation, and auditing logs.

---

**80. How do you upgrade an EKS cluster in AWS?**
- **Answer**: You can upgrade an Amazon EKS cluster by using the AWS Management Console, AWS CLI (`aws eks update

-cluster-version`), or by applying a new Kubernetes version through EKS updates.

---

**81. What is the difference between Deployment and ReplicaSet in Kubernetes?**
- **Answer**: A Deployment manages the lifecycle of Pods, allowing for easy updates, rollbacks, and scaling. A ReplicaSet ensures that a specified number of Pod replicas are running, but it doesn’t handle updates or rollbacks directly, which is the Deployment’s role.

---

**82. How do you define resource limits for a pod in Kubernetes?**
- **Answer**: Resource limits are defined in the pod’s spec file using `resources.requests` for minimum resources and `resources.limits` for maximum resources. This ensures that pods are allocated appropriate CPU and memory based on the cluster’s resource availability.

```yaml
resources:
  requests:
    cpu: 100m
    memory: 128Mi
  limits:
    cpu: 500m
    memory: 256Mi
```

---

**83. What is the difference between a ConfigMap and a Secret in Kubernetes?**
- **Answer**: A ConfigMap stores non-sensitive configuration data as key-value pairs, while a Secret stores sensitive information like passwords or API keys in an encoded format. Secrets provide additional security by encoding the data.

---

**84. How can you manually delete a pod in Kubernetes?**
- **Answer**: You can delete a pod using `kubectl delete pod <pod-name>`. If a ReplicaSet or Deployment manages the pod, Kubernetes will automatically recreate the pod.

---

**85. What is a Kubernetes Job, and when would you use it?**
- **Answer**: A Job in Kubernetes ensures that a pod runs to completion. It is used for tasks that need to be performed once, such as batch processing or database migrations.

---

**86. How do you perform a rolling update in Kubernetes?**
- **Answer**: A rolling update is done by updating a Deployment with a new version of the container image. Kubernetes incrementally updates the pods while ensuring that the desired number of replicas is always available.

```bash
kubectl set image deployment/<deployment-name> <container-name>=<new-image>
```

---

**87. What is the purpose of Kubernetes Ingress?**
- **Answer**: Kubernetes Ingress is used to manage external access to services in a cluster, typically HTTP. It provides load balancing, SSL termination, and path-based routing.

---

**88. How do you manage Kubernetes storage with Persistent Volumes (PV) and Persistent Volume Claims (PVC)?**
- **Answer**: Persistent Volumes (PVs) are provisioned by the administrator, while Persistent Volume Claims (PVCs) are made by users requesting storage. PVCs bind to PVs based on matching criteria like storage size and access modes.

---

**89. How can you expose a service in Kubernetes using NodePort?**
- **Answer**: A NodePort is used to expose a service on each node’s IP address on a specific port. This allows external access to a service running inside the cluster.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  type: NodePort
  selector:
    app: my-app
  ports:
    - port: 80
      nodePort: 30001
```

---

**90. What is Kubernetes’ Horizontal Pod Autoscaler (HPA)?**
- **Answer**: The HPA automatically scales the number of pod replicas based on CPU utilization or custom metrics. This ensures that your application can handle variable loads efficiently.

```bash
kubectl autoscale deployment <deployment-name> --cpu-percent=50 --min=1 --max=10
```

---

**91. How do you define a Kubernetes NetworkPolicy?**
- **Answer**: A NetworkPolicy allows you to control the traffic between pods and services. You can define rules to allow or block traffic based on namespaces, pods, or IPs.

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-app-traffic
spec:
  podSelector:
    matchLabels:
      app: my-app
  ingress:
  - from:
    - podSelector:
        matchLabels:
          app: allowed-app
```

---

**92. How do you set up a Kubernetes Service of type LoadBalancer?**
- **Answer**: A LoadBalancer service type provisions a cloud provider’s load balancer to route traffic to the pods. This is commonly used in cloud environments like AWS or GCP.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  type: LoadBalancer
  selector:
    app: my-app
  ports:
    - port: 80
      targetPort: 8080
```

---

**93. How do you configure multi-cloud Kubernetes clusters?**
- **Answer**: Multi-cloud Kubernetes clusters can be set up using tools like Rancher, or by using managed Kubernetes services (e.g., EKS, GKE, AKS) and federating clusters using Kubernetes Federation (KubeFed).

---

**94. What is the purpose of Kubernetes RBAC (Role-Based Access Control)?**
- **Answer**: RBAC allows you to control access to Kubernetes resources based on the roles assigned to users. This is done by creating roles and role bindings that specify permissions for resources.

---

**95. What are the differences between StatefulSet and Deployment in Kubernetes?**
- **Answer**: A Deployment manages stateless pods, while a StatefulSet is designed for stateful applications, providing stable identities and persistent storage across pod rescheduling.

---

**96. How do you monitor Kubernetes clusters?**
- **Answer**: Kubernetes clusters can be monitored using Prometheus for metrics collection, Grafana for visualization, and tools like Fluentd for logging.

---

**97. How does Istio integrate with Kubernetes?**
- **Answer**: Istio is a service mesh that provides advanced features like traffic management, security, and observability for microservices. It can be integrated with Kubernetes to manage communication between pods using sidecar proxies.

---

**98. What is the purpose of `kubectl describe` command in Kubernetes?**
- **Answer**: The `kubectl describe` command provides detailed information about Kubernetes resources such as pods, deployments, services, etc., including events, resource usage, and status.

---

**99. How do you scale a Kubernetes deployment?**
- **Answer**: Scaling a Kubernetes deployment can be done by adjusting the `replicas` field in the deployment YAML file or using `kubectl scale` command.

```bash
kubectl scale deployment <deployment-name> --replicas=5
```

---

**100. How do you perform a Kubernetes cluster upgrade?**
- **Answer**: Cluster upgrades can be done using `kubeadm` or by leveraging managed services (EKS, GKE, etc.). It involves upgrading control plane components and nodes while ensuring compatibility and maintaining uptime.

```bash
kubeadm upgrade apply <new-version>
```

---

**101. How do you perform a rolling update in Kubernetes?**
- **Answer**: A rolling update in Kubernetes can be done by updating the container image in a Deployment. Kubernetes gradually replaces the old pods with the new version while ensuring that the desired number of replicas is always available.

```bash
kubectl set image deployment/<deployment-name> <container-name>=<new-image>
```

---

**102. What are taints and tolerations in Kubernetes?**
- **Answer**: Taints are applied to nodes to prevent pods from being scheduled unless they tolerate the taint. Tolerations are applied to pods to allow them to be scheduled on tainted nodes.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  tolerations:
  - key: "key1"
    operator: "Equal"
    value: "value1"
    effect: "NoSchedule"
```

---

**103. How do you check the resource usage of a pod in Kubernetes?**
- **Answer**: You can check the resource usage of a pod by using the `kubectl top` command, which displays CPU and memory usage.

```bash
kubectl top pod <pod-name>
```

---

**104. What is the use of a Kubernetes ConfigMap?**
- **Answer**: A ConfigMap is used to store non-sensitive configuration data that can be injected into pods as environment variables, command-line arguments, or configuration files.

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: my-config
data:
  configKey: value
```

---

**105. What are the key differences between Kubernetes Pods and Containers?**
- **Answer**: A Pod is a Kubernetes abstraction that encapsulates one or more containers, networking, and storage resources, and it is the smallest deployable unit in Kubernetes. A container is a runtime environment for applications.

---

**106. How do you perform debugging of a failed pod in Kubernetes?**
- **Answer**: You can debug a failed pod by using `kubectl describe pod <pod-name>` to check for events or logs. Additionally, `kubectl logs <pod-name>` will help you view the logs of a specific container in the pod.

---

**107. What is a Kubernetes Service and how do you expose it?**
- **Answer**: A Kubernetes Service is an abstraction that defines a logical set of pods and a policy to access them. It can be exposed using types such as ClusterIP, NodePort, LoadBalancer, or ExternalName.

---

**108. What is the purpose of a Kubernetes Deployment?**
- **Answer**: A Deployment in Kubernetes manages the lifecycle of stateless applications. It ensures that the specified number of replicas of a pod are running at all times, and it provides easy updates and rollbacks.

---

**109. How do you configure Horizontal Pod Autoscaling (HPA) in Kubernetes?**
- **Answer**: HPA automatically scales the number of pod replicas based on CPU usage or custom metrics. You can configure it using `kubectl autoscale` or by creating an HPA resource in a YAML file.

```bash
kubectl autoscale deployment <deployment-name> --cpu-percent=50 --min=1 --max=10
```

---

**110. What is a StatefulSet and when would you use it?**
- **Answer**: A StatefulSet is used for stateful applications in Kubernetes. It ensures that pods have unique, persistent identities and stable storage, which is important for applications like databases that require stable network identities.

---

**111. What is a PodDisruptionBudget in Kubernetes?**
- **Answer**: A PodDisruptionBudget (PDB) ensures that a minimum number of pods in a deployment or replica set are available during voluntary disruptions, such as node upgrades or rolling updates.

```yaml
apiVersion: policy/v1
kind: PodDisruptionBudget
metadata:
  name: my-pdb
spec:
  minAvailable: 1
  selector:
    matchLabels:
      app: my-app
```

---

**112. How does Kubernetes handle secret management?**
- **Answer**: Kubernetes manages secrets using the `Secret` resource, which stores sensitive data such as passwords and tokens in an encrypted format. These secrets can be injected into pods as environment variables or volumes.

---

**113. What is a Kubernetes CronJob and how do you use it?**
- **Answer**: A CronJob runs jobs at scheduled times, similar to cron jobs in Linux. It's useful for tasks like backups, cleanup jobs, or batch processing.

```yaml
apiVersion: batch/v1
kind: CronJob
metadata:
  name: my-cronjob
spec:
  schedule: "*/5 * * * *"
  jobTemplate:
    spec:
      template:
        spec:
          containers:
          - name: my-container
            image: my-image
```

---

**114. How do you manage Kubernetes version upgrades?**
- **Answer**: Kubernetes version upgrades can be performed using `kubeadm` or by leveraging managed Kubernetes services like EKS, GKE, or AKS. Upgrade control plane components and then worker nodes while ensuring compatibility.

---

**115. How do you configure persistent storage for stateful applications in Kubernetes?**
- **Answer**: Persistent storage is managed using Persistent Volumes (PV) and Persistent Volume Claims (PVC). PVCs request storage resources, and PVs provide them.

---

**116. How do you implement network policies in Kubernetes?**
- **Answer**: Network policies define how pods communicate with each other. You can use them to restrict traffic to and from pods based on labels, namespaces, or IP ranges.

---

**117. How do you manage deployments with multiple containers in Kubernetes?**
- **Answer**: You can use multi-container pods where each container runs in the same pod, sharing the same network namespace. Use different containers to handle different parts of the application, such as logging or monitoring.

---

**118. How do you monitor Kubernetes cluster health and resource usage?**
- **Answer**: Kubernetes cluster health and resource usage can be monitored using Prometheus, Grafana, and custom metrics. Tools like `kubectl top nodes/pods` can also be used for quick insights.

---

**119. How do you handle rolling back a failed deployment in Kubernetes?**
- **Answer**: You can roll back a deployment using `kubectl rollout undo`. It will revert the deployment to its previous version.

```bash
kubectl rollout undo deployment/<deployment-name>
```

---

**120. How do you update a Kubernetes deployment to use a new image?**
- **Answer**: Update the deployment by using `kubectl set image` to point to the new image. Kubernetes will automatically perform a rolling update for you.

```bash
kubectl set image deployment/<deployment-name> <container-name>=<new-image>
```

---

**121. What are Init Containers in Kubernetes, and how are they used?**  
- **Answer**: Init containers are special containers that run before the main application containers in a pod. They are used to perform initialization tasks, such as waiting for a service to be ready or fetching configuration files. Init containers ensure that the main application container runs in a properly configured environment.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: init-container-example
spec:
  initContainers:
  - name: init-container
    image: busybox
    command: ['sh', '-c', 'echo Initializing && sleep 5']
  containers:
  - name: main-container
    image: nginx
```

---

**122. What is the difference between NodePort, ClusterIP, and LoadBalancer services?**  
- **Answer**:  
  - **NodePort**: Exposes the service on a specific port of each node in the cluster, making it accessible externally.  
  - **ClusterIP**: The default service type, accessible only within the cluster using the cluster's internal IP.  
  - **LoadBalancer**: Creates an external load balancer and routes traffic to the service pods.  

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  type: NodePort  # or ClusterIP or LoadBalancer
  ports:
  - port: 80
    targetPort: 8080
    nodePort: 30007
```

---

**123. How do you configure Horizontal Pod Autoscaling (HPA) with custom metrics?**  
- **Answer**: To use custom metrics, you need to set up a custom metrics server like Prometheus Adapter and configure HPA to use the metrics.

```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: custom-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: my-deployment
  minReplicas: 1
  maxReplicas: 10
  metrics:
  - type: Pods
    pods:
      metric:
        name: custom_metric
      target:
        type: AverageValue
        averageValue: 500m
```

---

**124. What is the difference between ReplicaSet and Deployment?**  
- **Answer**:  
  - **ReplicaSet**: Ensures a specified number of pod replicas are running at all times. It is a lower-level resource.
  - **Deployment**: Provides declarative updates for pods and ReplicaSets, supporting rolling updates and rollbacks. It is the recommended way to manage stateless applications.

---

**125. How do you troubleshoot a CrashLoopBackOff error in a pod?**  
- **Answer**:  
  1. Check the pod's events:
     ```bash
     kubectl describe pod <pod-name>
     ```
  2. View the pod logs:
     ```bash
     kubectl logs <pod-name>
     ```
  3. Check resource limits, liveness/readiness probes, and container entrypoints for issues.

---

**126. What are Kubernetes DaemonSets used for?**  
- **Answer**: DaemonSets ensure that a copy of a pod runs on all (or some) nodes in the cluster. They are typically used for logging, monitoring, or networking services like Fluentd, Prometheus Node Exporter, or Calico.

```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: my-daemonset
spec:
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: my-container
        image: my-image
```

---

**127. What is Istio, and how is it used with Kubernetes?**  
- **Answer**: Istio is a service mesh that provides traffic management, security, and observability for microservices. It runs alongside your Kubernetes cluster and manages service-to-service communication.

---

**128. How do you configure Kubernetes Ingress for HTTPS?**  
- **Answer**: To enable HTTPS, you need to create a TLS certificate and associate it with your ingress resource.

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: https-ingress
spec:
  tls:
  - hosts:
    - my-app.example.com
    secretName: tls-secret
  rules:
  - host: my-app.example.com
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

**129. How do you drain a Kubernetes node for maintenance?**  
- **Answer**: Use the `kubectl drain` command to safely evict all pods from the node.

```bash
kubectl drain <node-name> --ignore-daemonsets
```

---

**130. What is the purpose of Kubernetes Labels and Selectors?**  
- **Answer**: Labels are key-value pairs attached to Kubernetes objects to organize and select subsets of objects. Selectors enable you to filter objects based on labels.

---

**131. How do you upgrade a Kubernetes cluster using kubeadm?**  
- **Answer**:  
  1. Upgrade the kubeadm tool on the control plane node.
     ```bash
     apt-get update && apt-get install -y kubeadm
     kubeadm upgrade plan
     kubeadm upgrade apply <version>
     ```
  2. Upgrade kubelet and kubectl on all nodes.
     ```bash
     apt-get install -y kubelet kubectl
     systemctl restart kubelet
     ```

---

**132. How do you manage secrets securely in Kubernetes?**  
- **Answer**: Kubernetes stores secrets in base64-encoded format. To improve security, enable encryption at rest in the `kube-apiserver` configuration.

```bash
kubectl create secret generic my-secret --from-literal=password=Pa$$w0rd
```

---

**133. What is a Kubernetes PersistentVolume (PV) and PersistentVolumeClaim (PVC)?**  
- **Answer**:  
  - PV: Represents physical storage in the cluster.  
  - PVC: Requests storage resources from a PV.  

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: my-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
```

---

**134. How do you configure Node Affinity in Kubernetes?**  
- **Answer**: Node affinity schedules pods on specific nodes based on labels.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: affinity-pod
spec:
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: node-type
            operator: In
            values:
            - gpu
```

---

**135. What are Kubernetes Readiness and Liveness Probes?**  
- **Answer**:  
  - **Readiness Probe**: Determines if the pod is ready to accept traffic.  
  - **Liveness Probe**: Checks if the pod is alive; restarts it if not.

```yaml
readinessProbe:
  httpGet:
    path: /healthz
    port: 8080
livenessProbe:
  tcpSocket:
    port: 8080
```

---
Continuing with **Kubernetes Questions** from 136 onward:  

---

**136. What is a Kubernetes Job, and how is it different from a Deployment?**  
- **Answer**: A Job is used to create one or more pods that run to completion, typically for batch or scheduled tasks. Unlike Deployments, Jobs do not run indefinitely and are not meant for services that require high availability.  

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: example-job
spec:
  template:
    spec:
      containers:
      - name: example
        image: busybox
        command: ["echo", "Hello Kubernetes!"]
      restartPolicy: Never
  backoffLimit: 4
```

---

**137. How do you monitor resource usage of Kubernetes pods?**  
- **Answer**: Use `kubectl top` to monitor pod resource usage.  
```bash
kubectl top pods --namespace=<namespace>
```
Alternatively, use tools like **Prometheus** and **Grafana** for advanced monitoring.

---

**138. What are Resource Requests and Limits in Kubernetes, and why are they important?**  
- **Answer**: Resource requests specify the minimum resources a container needs, while limits specify the maximum it can use. These help in optimizing cluster resource allocation and avoiding resource contention.  

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

**139. How do you configure a Kubernetes Horizontal Pod Autoscaler (HPA)?**  
- **Answer**: HPA scales the number of pods based on CPU/memory usage or custom metrics.  

```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: hpa-example
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: my-deployment
  minReplicas: 2
  maxReplicas: 10
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 50
```

---

**140. What are StatefulSets, and when should you use them?**  
- **Answer**: StatefulSets are used for managing stateful applications like databases. They maintain a stable network identity and storage across pod restarts.  

Use cases include databases like MySQL, PostgreSQL, or distributed systems like Cassandra.  

```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: example-statefulset
spec:
  serviceName: "nginx"
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
```

---

**141. What is the purpose of Kubernetes Taints and Tolerations?**  
- **Answer**: Taints prevent pods from being scheduled on specific nodes unless the pod has a matching toleration. This is useful for isolating workloads.  

```yaml
# Adding a taint to a node
kubectl taint nodes node1 key=value:NoSchedule

# Adding a toleration to a pod
tolerations:
- key: "key"
  operator: "Equal"
  value: "value"
  effect: "NoSchedule"
```

---

**142. What is Pod Disruption Budget (PDB), and how is it configured?**  
- **Answer**: PDB ensures a minimum number of pods remain available during voluntary disruptions like node upgrades.  

```yaml
apiVersion: policy/v1
kind: PodDisruptionBudget
metadata:
  name: pdb-example
spec:
  minAvailable: 2
  selector:
    matchLabels:
      app: my-app
```

---

**143. How do you debug a pod stuck in the Pending state?**  
- **Answer**:  
  1. Check pod events:
     ```bash
     kubectl describe pod <pod-name>
     ```
  2. Verify resource availability:
     ```bash
     kubectl get nodes --output wide
     ```
  3. Inspect scheduling issues (node selectors, tolerations, etc.).

---

**144. What is the difference between Ingress and Egress in Kubernetes?**  
- **Answer**:  
  - **Ingress**: Controls incoming traffic to the cluster, routing it to services.  
  - **Egress**: Controls outgoing traffic from the cluster to external systems.

---

**145. What are Kubernetes ConfigMaps, and how do you use them?**  
- **Answer**: ConfigMaps store non-sensitive configuration data that can be injected into pods as environment variables or mounted as files.  

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  config.json: |
    {
      "name": "app-name",
      "version": "1.0"
    }
```

---

**146. How do you configure a Network Policy in Kubernetes?**  
- **Answer**: Network policies control the ingress and egress traffic for pods based on labels.  

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-app
spec:
  podSelector:
    matchLabels:
      app: my-app
  ingress:
  - from:
    - podSelector:
        matchLabels:
          role: frontend
```

---

**147. What are CRDs (Custom Resource Definitions) in Kubernetes?**  
- **Answer**: CRDs extend Kubernetes by allowing users to define custom resources. These are useful for domain-specific objects like ArgoCD applications.

```yaml
apiVersion: apiextensions.k8s.io/v1
kind: CustomResourceDefinition
metadata:
  name: myresources.mygroup.example.com
spec:
  group: mygroup.example.com
  names:
    plural: myresources
    singular: myresource
    kind: MyResource
  scope: Namespaced
  versions:
  - name: v1
    served: true
    storage: true
```

---

**148. How do you troubleshoot high CPU usage in a pod?**  
- **Answer**:  
  1. Check metrics using `kubectl top pods`.  
  2. Analyze logs:  
     ```bash
     kubectl logs <pod-name>
     ```
  3. Monitor using Prometheus and Grafana dashboards.  
  4. Check resource requests and limits in pod configuration.

---

**149. How do you roll back a Deployment in Kubernetes?**  
- **Answer**: Use the `kubectl rollout` command.  
```bash
kubectl rollout undo deployment <deployment-name>
```

---

**150. What is the difference between Rolling Updates and Blue-Green Deployments in Kubernetes?**  
- **Answer**:  
  - **Rolling Updates**: Gradually replace old pods with new ones to ensure zero downtime.  
  - **Blue-Green Deployments**: Maintain two environments (blue and green), switching traffic to the green (new) environment once it's ready.  

---
