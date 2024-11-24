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
