### Docker Questions: Set 1 (Basics to Intermediate)

#### **1. What is Docker?**
- **Definition**: Docker is an open-source platform that automates the deployment of applications inside lightweight, portable containers.  
- **Purpose**:  
  - Ensures consistency across multiple environments (development, testing, production).  
  - Simplifies the process of software delivery.  
- **Example**: Think of Docker containers as "shipping containers" for software—everything needed to run an app (code, libraries, dependencies) is packaged inside.

---

#### **2. What is a Docker Container?**
- **Definition**: A container is a lightweight, standalone, and executable software package that includes everything needed to run an application.  
- **Purpose**: Runs consistently on any system with Docker installed.  
- **Example**:  
  - Create a container for a Python app:  
    ```bash
    docker run python:3.8 python -c "print('Hello, World!')"
    ```  
  - This command runs Python code inside a Docker container.

---

#### **3. What is the difference between a VM and a Docker Container?**
| **Feature**       | **Virtual Machine (VM)**       | **Docker Container**        |
|--------------------|--------------------------------|-----------------------------|
| **Size**           | Large (GBs)                  | Lightweight (MBs)           |
| **Startup Time**   | Minutes                      | Seconds                     |
| **OS Dependency**  | Full OS included             | Shares host OS kernel       |
| **Performance**    | High overhead                | Minimal overhead            |

---

#### **4. What are Docker Images?**
- **Definition**: A Docker image is a lightweight, immutable file containing instructions to create a Docker container.  
- **Purpose**: Acts as a blueprint for containers.  
- **Example**:  
  - Pull an image from Docker Hub:  
    ```bash
    docker pull nginx
    ```  

---

#### **5. What is the difference between `COPY` and `ADD` in Dockerfile?**
| **Feature**      | **COPY**                       | **ADD**                     |
|-------------------|--------------------------------|-----------------------------|
| **Purpose**       | Copies files/directories      | Copies files + extracts archives |
| **Use Case**      | Simpler, predictable          | Supports tar extraction and URL fetching |
| **Example**       |  
  - `COPY`:  
    ```dockerfile
    COPY index.html /usr/share/nginx/html
    ```  
  - `ADD`:  
    ```dockerfile
    ADD website.tar.gz /usr/share/nginx/html
    ```  

---

#### **6. Explain the difference between `RUN`, `CMD`, and `ENTRYPOINT` in Dockerfile.**
| **Instruction**   | **Purpose**                                   | **Example**               |
|-------------------|-----------------------------------------------|---------------------------|
| **RUN**           | Executes commands during build time.          | `RUN apt-get update`      |
| **CMD**           | Default command to execute at runtime.        | `CMD ["python", "app.py"]` |
| **ENTRYPOINT**    | Configures a container to run as an executable. | `ENTRYPOINT ["nginx"]`    |

---

#### **7. What is Docker Compose, and why is it used?**
- **Definition**: Docker Compose is a tool to define and manage multi-container applications using a YAML file.  
- **Purpose**: Simplifies running and managing applications with multiple services.  
- **Example**:  
  - `docker-compose.yml` for a web app with a database:  
    ```yaml
    version: "3.8"
    services:
      web:
        image: nginx
        ports:
          - "80:80"
      db:
        image: mysql
        environment:
          MYSQL_ROOT_PASSWORD: password
    ```  

---

#### **8. How do you expose ports in Docker?**
- **Command**: Use the `-p` flag in `docker run` to map container ports to host ports.  
- **Example**:  
  - Run Nginx and expose it on port 8080:  
    ```bash
    docker run -p 8080:80 nginx
    ```  
  - This maps port 80 in the container to port 8080 on the host.

---

#### **9. Explain Docker Volumes and their types.**
- **Definition**: Docker Volumes are used to persist data generated by a container.  
- **Types**:  
  - **Anonymous Volumes**: Automatically created by Docker.  
  - **Named Volumes**: Created explicitly using Docker commands.  
  - **Bind Mounts**: Map a host directory to a container directory.  
- **Example**:  
  - Create and use a named volume:  
    ```bash
    docker volume create mydata
    docker run -v mydata:/data myapp
    ```  

---

#### **10. What are the common Docker Networking Modes?**
| **Mode**        | **Purpose**                                      |
|------------------|--------------------------------------------------|
| **Bridge**       | Default mode; containers can communicate internally. |
| **Host**         | Shares the host network namespace.               |
| **None**         | No network is assigned to the container.         |
| **Overlay**      | For multi-host communication using Swarm.        |

---

#### **11. What is a Multi-Stage Build in Docker?**
- **Definition**: A feature in Dockerfile that allows creating lightweight images by using intermediate stages for building dependencies.  
- **Example**:  
  ```dockerfile
  FROM node:18 as builder
  WORKDIR /app
  COPY . .
  RUN npm install && npm run build

  FROM nginx
  COPY --from=builder /app/build /usr/share/nginx/html
  ```

---

#### **12. How do you monitor running Docker containers?**
- Use `docker stats` to monitor CPU, memory, and network usage.  
- **Command**:  
  ```bash
  docker stats
  ```  

---

#### **13. How do you ensure Docker image security?**
- Use trusted base images from official repositories.  
- Regularly scan images using tools like **Trivy** or **Docker Scan**.  
- Implement multi-stage builds to minimize attack surface.

---

#### **14. How does Docker networking work?**
- Docker uses **virtual bridges** to connect containers.  
- Containers on the same bridge can communicate directly, while external traffic is routed through NAT.

---

#### **15. Real-Time Issue: Container Crashes Frequently**  
- **Cause**: Incorrect configurations, missing dependencies, or high resource usage.  
- **Solution**:  
  - Inspect logs:  
    ```bash
    docker logs <container_id>
    ```  
  - Check resource usage with `docker stats`.  
  - Adjust resource limits in Docker Compose or Kubernetes.

---

#### **16. Real-Time Issue: Containers Cannot Communicate**  
- **Cause**: Networking misconfiguration or firewall issues.  
- **Solution**:  
  - Check network mode:  
    ```bash
    docker network inspect <network_name>
    ```  
  - Use `bridge` or `host` networking appropriately.

Here is the continuation of the Docker questions:

---

#### **17. How do you use Docker Compose to manage multi-container applications?**
- **Definition**: Docker Compose is used to define and run multi-container applications by using a `docker-compose.yml` file. It allows you to start, stop, and rebuild services in an isolated environment.
- **Example**:  
  - A `docker-compose.yml` file for a basic web app and database:  
    ```yaml
    version: "3.8"
    services:
      web:
        image: nginx
        ports:
          - "8080:80"
      db:
        image: mysql
        environment:
          MYSQL_ROOT_PASSWORD: password
    ```
  - To start the containers:  
    ```bash
    docker-compose up
    ```

---

#### **18. How do you configure persistent storage in Docker?**
- **Definition**: Docker uses volumes to persist data outside of the container lifecycle, preventing data loss when a container is removed or recreated.
- **Types of Volumes**: 
  - **Named Volumes**: Explicitly named volumes stored in Docker's volume storage.
  - **Bind Mounts**: Bind a host directory to a container directory.
- **Example**:  
  - Create a named volume and use it:  
    ```bash
    docker volume create my_volume
    docker run -v my_volume:/data my_image
    ```

---

#### **19. What is the use of Docker `EXPOSE` instruction in Dockerfile?**
- **Definition**: The `EXPOSE` instruction informs Docker that the container listens on the specified network ports at runtime. It does not actually publish the port.
- **Purpose**: It helps document which ports the container will use and can be used by tools like `docker-compose`.
- **Example**:  
  ```dockerfile
  EXPOSE 80
  ```
  - This makes the container listen on port 80 but doesn't publish it.

---

#### **20. How can you share data between containers?**
- **Definition**: Docker uses volumes or networks to share data between containers. 
- **Use Case**: Volumes are often used when data needs to persist beyond the container lifecycle, while networks facilitate communication between containers.
- **Example**:  
  - **Volumes**:  
    ```bash
    docker run -v shared_data:/data container1
    docker run -v shared_data:/data container2
    ```  
  - **Network**:  
    ```bash
    docker network create my_network
    docker run --network=my_network container1
    docker run --network=my_network container2
    ```

---

#### **21. What is Docker's default network driver?**
- **Definition**: Docker uses the `bridge` network driver by default.
- **Purpose**: It creates a private internal network on your host system, where containers can communicate with each other.
- **Example**:  
  - Start a container using the default bridge network:  
    ```bash
    docker run --name web --network bridge nginx
    ```

---

#### **22. What are Docker namespaces and cgroups?**
- **Namespaces**: Provide isolation for containers, giving each container its own filesystem, network interfaces, process IDs, etc.
- **Cgroups**: Limit and prioritize the resources (CPU, memory) available to containers.
- **Example**:  
  - **Namespaces**: Ensures containers are isolated from each other.  
  - **Cgroups**: Restricts CPU and memory usage, preventing one container from consuming all host resources.

---

#### **23. What is Docker Swarm?**
- **Definition**: Docker Swarm is Docker's native clustering and orchestration tool. It allows you to manage a cluster of Docker nodes (machines) and deploy containers across them.
- **Purpose**: Enables scaling, load balancing, and fault tolerance for Docker containers.
- **Example**:  
  - Initialize a Swarm:  
    ```bash
    docker swarm init
    ```
  - Deploy a service in the Swarm:  
    ```bash
    docker service create --name my_service --replicas 3 nginx
    ```

---

#### **24. How do you build Docker images from a Dockerfile?**
- **Definition**: You can build Docker images by using the `docker build` command, which processes the instructions in a `Dockerfile`.
- **Command**:  
  ```bash
  docker build -t my_image .
  ```
- **Explanation**: This command builds the image based on the `Dockerfile` in the current directory (denoted by `.`).

---

#### **25. What is the difference between `docker exec` and `docker attach`?**
| **Command**       | **Purpose**                                      |
|-------------------|--------------------------------------------------|
| **docker exec**   | Runs a new command inside a running container.   |
| **docker attach** | Attaches to an already running container's process. |

- **Example**:  
  - Run a command in a running container:  
    ```bash
    docker exec -it container_id bash
    ```  
  - Attach to a running container:  
    ```bash
    docker attach container_id
    ```

---

#### **26. How do you manage multiple containers in Docker Compose?**
- **Definition**: Docker Compose allows you to define and run multi-container Docker applications with the use of a single `docker-compose.yml` file.
- **Purpose**: It simplifies container orchestration for development and testing environments.
- **Example**:  
  - Start multiple containers:  
    ```bash
    docker-compose up
    ```
  - Define multiple services in `docker-compose.yml`:  
    ```yaml
    version: "3.8"
    services:
      web:
        image: nginx
      db:
        image: mysql
        environment:
          MYSQL_ROOT_PASSWORD: password
    ```

---

#### **27. How do you manage Docker in production?**
- **Explanation**: Use orchestration tools like Kubernetes or Docker Swarm to manage containers at scale, ensuring high availability, scaling, and fault tolerance.  
- **Best Practices**:
  - Monitor containers with tools like **Prometheus**, **Grafana**, or **Docker stats**.
  - Automate deployment with CI/CD pipelines using tools like **Jenkins**.
  - Use Docker secrets for managing sensitive data.

---

#### **28. How do you ensure image security?**
- **Best Practices**:  
  - Use official images from trusted sources.  
  - Regularly scan images for vulnerabilities using tools like **Trivy** or **Docker Scan**.  
  - Follow best practices for Dockerfile security to minimize the attack surface.  
- **Example**:  
  - Scan image with Docker Scan:  
    ```bash
    docker scan my_image
    ```

---

#### **29. How does Docker handle environment variables?**
- **Definition**: Docker can pass environment variables to containers at runtime using the `-e` flag or via a `.env` file.
- **Example**:  
  - Passing an environment variable:  
    ```bash
    docker run -e MY_ENV_VAR=value my_image
    ```  
  - Use `.env` file to load variables:  
    ```bash
    docker-compose --env-file .env up
    ```

---

#### **30. What is the `docker network` command and how is it used?**
- **Definition**: The `docker network` command is used to manage Docker networks, allowing you to create, inspect, and remove networks.
- **Purpose**: It allows you to manage container communication by creating different network modes (bridge, host, overlay).
- **Example**:  
  - Create a custom bridge network:  
    ```bash
    docker network create my_network
    ```
  - Connect a container to the network:  
    ```bash
    docker run --network my_network my_image
    ```

---
Here’s the continuation of the Docker questions focusing on networking, advanced issues, and real-time scenarios:

---

#### **31. What are Docker networks and why do you need them?**
- **Definition**: Docker networks allow containers to communicate with each other and with the outside world. Containers in the same network can easily communicate without exposing ports to the host.
- **Types of Docker Networks**:
  - **Bridge**: Default network driver for standalone containers.
  - **Host**: Containers share the host network stack.
  - **Overlay**: Used for multi-host networking in Docker Swarm.
  - **Macvlan**: Assigns a MAC address to containers, making them appear as physical devices on the network.
- **Example**:
  - Create a custom bridge network:  
    ```bash
    docker network create my_bridge_network
    ```
  - Connect a container to the network:  
    ```bash
    docker run --network my_bridge_network my_image
    ```

---

#### **32. What is the difference between `docker run` and `docker exec`?**
| **Command**        | **Purpose**                                         |
|--------------------|-----------------------------------------------------|
| **docker run**     | Used to create and start a new container.           |
| **docker exec**    | Runs commands in an already running container.      |

- **Example**:
  - Run a new container:  
    ```bash
    docker run -d --name my_container my_image
    ```
  - Execute a command in a running container:  
    ```bash
    docker exec -it my_container bash
    ```

---

#### **33. How do you troubleshoot container networking issues?**
- **Common Issues**: Containers cannot communicate, IP address conflicts, DNS issues within containers.
- **Solutions**:  
  - Use `docker network inspect` to check network details.
  - Check the `docker ps` and `docker logs` for container-specific issues.
  - Inspect the container’s network interfaces:  
    ```bash
    docker exec -it container_id ip a
    ```

---

#### **34. How do Docker volumes handle persistent data?**
- **Definition**: Docker volumes are used to persist data created by and used by Docker containers. Volumes are stored outside of containers and ensure data is not lost when containers are removed.
- **Example**:
  - Create and mount a volume:  
    ```bash
    docker volume create my_volume
    docker run -v my_volume:/data my_image
    ```

---

#### **35. What is the purpose of Docker Compose?**
- **Definition**: Docker Compose is a tool for defining and running multi-container Docker applications. It uses a `docker-compose.yml` file to configure services, networks, and volumes.
- **Example**:
  - A sample `docker-compose.yml` for a web application:
    ```yaml
    version: "3.8"
    services:
      web:
        image: nginx
        ports:
          - "8080:80"
      db:
        image: mysql
        environment:
          MYSQL_ROOT_PASSWORD: password
    ```
  - Start the containers:
    ```bash
    docker-compose up
    ```

---

#### **36. How do you handle Docker container scaling?**
- **Definition**: Docker containers can be scaled using orchestration tools like Docker Swarm or Kubernetes. You can scale the number of containers running a service by increasing the replica count.
- **Example**:
  - Scale a service in Docker Swarm:
    ```bash
    docker service scale my_service=5
    ```

---

#### **37. How do you upgrade a running Docker container?**
- **Definition**: To upgrade a running container, you need to stop and remove the existing container, pull the new image, and then recreate the container.
- **Example**:
  - Stop and remove the container:
    ```bash
    docker stop my_container
    docker rm my_container
    ```
  - Pull the new image and run the container:
    ```bash
    docker pull my_image:latest
    docker run -d --name my_container my_image:latest
    ```

---

#### **38. How do you create a custom Docker network?**
- **Definition**: Docker allows you to create custom networks that enable better isolation and control over communication between containers.
- **Example**:
  - Create a custom bridge network:
    ```bash
    docker network create --driver bridge my_custom_network
    ```
  - Connect containers to the custom network:
    ```bash
    docker run --network my_custom_network my_image
    ```

---

#### **39. How does Docker handle DNS resolution within containers?**
- **Definition**: Docker uses an internal DNS server to resolve container names to IP addresses, allowing containers on the same network to communicate by name.
- **Example**:
  - If you have two containers `web` and `db`, the `web` container can access `db` by using the hostname `db`.

---

#### **40. What is the difference between `ADD` and `COPY` in a Dockerfile?**
| **Instruction**  | **Purpose**                                                         | **Use Case**                                             |
|------------------|---------------------------------------------------------------------|---------------------------------------------------------|
| **ADD**          | Copies files from the host system into the container and can also handle remote URLs and compressed files. | Use when you need to handle compressed files or URLs. |
| **COPY**         | Simply copies files or directories from the host system to the container. | Use for copying local files to the container.         |

- **Example**:
  - Using `ADD` to add a tar file:
    ```dockerfile
    ADD myapp.tar.gz /app
    ```
  - Using `COPY` to add a file:
    ```dockerfile
    COPY ./app /app
    ```

---

#### **41. What is Docker’s Layered Architecture?**
- **Definition**: Docker images are built in layers, where each layer represents a change to the image, such as installing software or adding files. Layers are cached to optimize builds.
- **Example**:
  - Dockerfile that adds layers:
    ```dockerfile
    FROM ubuntu
    RUN apt-get update
    COPY . /app
    ```

---

#### **42. What are Docker "tags"?**
- **Definition**: A Docker tag is a label given to a specific image version, allowing you to distinguish between different versions of an image.
- **Example**:
  - Tagging an image:
    ```bash
    docker tag my_image:latest my_image:v1.0
    ```

---

#### **43. What is the purpose of the `docker ps` command?**
- **Definition**: `docker ps` lists all running containers. Use it to check the status and other details of your active containers.
- **Example**:
  ```bash
  docker ps
  ```

---

#### **44. How do you inspect a Docker container?**
- **Definition**: `docker inspect` provides detailed information about a container’s configuration and state.
- **Example**:
  ```bash
  docker inspect my_container
  ```

---

#### **45. How do you remove unused Docker images?**
- **Definition**: Docker images that are not in use can be cleaned up to free up disk space. The `docker image prune` command removes all unused images.
- **Example**:
  ```bash
  docker image prune -a
  ```

---

#### **46. What are Docker Multi-stage builds?**
- **Definition**: Multi-stage builds allow you to use multiple `FROM` statements in a single Dockerfile to optimize the final image by only including the necessary artifacts.
- **Example**:
  ```dockerfile
  FROM node:14 AS build
  WORKDIR /app
  COPY . .
  RUN npm install

  FROM nginx:alpine
  COPY --from=build /app/dist /usr/share/nginx/html
  ```

---

#### **47. How do you ensure consistency between development, testing, and production environments using Docker?**
- **Definition**: Docker containers ensure consistency by providing isolated, reproducible environments that can run the same code across all stages of the development lifecycle.
- **Example**:  
  - Ensure that the same Docker image used for development is used in testing and production, avoiding environment-specific issues.

---

#### **48. What is the use of Docker `CMD` instruction in a Dockerfile?**
- **Definition**: The `CMD` instruction specifies the command that will be executed when the container is run. Unlike `RUN`, `CMD` is not executed during image build time but when the container starts.
- **Example**:  
  ```dockerfile
  CMD ["nginx", "-g", "daemon off;"]
  ```

---

#### **49. How do you troubleshoot container logs in Docker?**
- **Definition**: You can use the `docker logs` command to access the logs generated by a container. This is helpful for debugging issues within a container.
- **Example**:
  ```bash
  docker logs my_container
  ```

---

#### **50. What are Docker Secrets and how do they work?**
- **Definition**: Docker secrets are used to manage sensitive information, such as passwords and API keys, in a secure way within Docker Swarm.
- **Example**:  
  - Create a secret and use it in a container:
    ```bash
    echo "my_secret" | docker secret create my_secret -
    docker service create --name my_service --secret my_secret nginx
    ```

---

**51. What are the differences between `CMD` and `ENTRYPOINT` in Docker?**
- **CMD**: Defines the default command to run when the container starts.
- **ENTRYPOINT**: Defines the default executable and cannot be overridden.
  
**Example**:
```dockerfile
CMD ["nginx"]
ENTRYPOINT ["nginx", "-g", "daemon off;"]
```
- **Difference**: If both are used, `ENTRYPOINT` is executed first, followed by `CMD`.

---

**52. What is the purpose of Docker health checks?**
- **Definition**: A health check in Docker allows you to check the status of your application running inside the container. It's a way to verify if the container is healthy and performing as expected.
  
**Example**:
```dockerfile
HEALTHCHECK CMD curl --fail http://localhost:8080/ || exit 1
```

---

**53. What is the Docker registry?**
- **Definition**: Docker registry is a storage and distribution system for Docker images. Docker Hub is a public registry, and you can also set up your own private registry.

---

**54. How do you clean up unused Docker images, containers, and volumes?**
- **Answer**: Use the `docker system prune` command to clean up unused images, containers, networks, and volumes.
  
**Example**:
```bash
docker system prune -a
```

---

**55. What is the difference between `docker volume` and `docker bind mount`?**
| **Aspect**              | **Docker Volume**                              | **Docker Bind Mount**                              |
|-------------------------|-----------------------------------------------|--------------------------------------------------|
| **Storage location**    | Stored in Docker's managed directory          | Stored on the host filesystem                     |
| **Persistence**         | Data persists even after container stops      | Data is lost if the container is removed          |
| **Use Case**            | For persistent data shared across containers  | For binding specific files or directories        |

---

**56. How do you network containers in Docker?**
- **Answer**: Containers are networked using Docker's built-in network drivers like `bridge`, `host`, `overlay`, and `macvlan`.

---

**57. How do you ensure that a Docker container starts automatically after a system reboot?**
- **Answer**: Use the `--restart` option in `docker run` to set automatic restart policies like `no`, `always`, `unless-stopped`, or `on-failure`.

**Example**:
```bash
docker run --restart always my_image
```

---

**58. What is the difference between `docker exec` and `docker attach`?**
| **Command**            | **docker exec**                                  | **docker attach**                                    |
|------------------------|---------------------------------------------------|------------------------------------------------------|
| **Purpose**            | Runs commands in a running container              | Connects to the running container's primary process |
| **Use Case**           | Running one-off commands in a container           | Debugging a running container's main process         |

---

**59. What is Docker Swarm, and how does it relate to Kubernetes?**
- **Docker Swarm**: Docker's native orchestration tool for managing a cluster of Docker nodes.
- **Kubernetes**: A more feature-rich container orchestration platform that supports advanced use cases for scaling, self-healing, and fault tolerance.

---

**60. How do you secure Docker images?**
- **Answer**:
  - Use trusted base images (e.g., official images from Docker Hub).
  - Regularly scan images for vulnerabilities using tools like Trivy or Clair.
  - Minimize the number of layers and sensitive data in the image.
  
---

**61. How do you use multi-stage builds in Docker?**
- **Answer**: Multi-stage builds allow you to create smaller and more efficient Docker images by separating the build and runtime environments into different stages.
  
**Example**:
```dockerfile
# Build stage
FROM node:18 AS build
WORKDIR /app
COPY . .
RUN npm install && npm run build

# Production stage
FROM nginx:alpine
COPY --from=build /app/build /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

---

**62. How do you limit Docker container resource usage (memory, CPU)?**
- **Answer**: You can limit the memory and CPU usage of a container using the `--memory` and `--cpus` flags.

**Example**:
```bash
docker run --memory="1g" --cpus="0.5" my_image
```

---

**63. How do you add metadata to a Docker image?**
- **Answer**: Use the `LABEL` instruction in the Dockerfile to add metadata.
  
**Example**:
```dockerfile
LABEL version="1.0" description="My custom Docker image"
```

---

**64. What is Docker Compose and why is it useful?**
- **Answer**: Docker Compose is a tool for defining and running multi-container Docker applications. It allows you to configure all your services (like frontend, backend, database) in one file and manage them with simple commands.

---

**65. How do you make a Docker container interactive?**
- **Answer**: You can make a container interactive using the `-it` flag, which allows you to run the container in interactive mode with a terminal.

**Example**:
```bash
docker run -it ubuntu bash
```

---

**66. How do you create a Docker image from an existing container?**
- **Answer**: You can create an image from a running container using `docker commit`.

**Example**:
```bash
docker commit my_container my_image:v1
```

---

**67. How do you run a Docker container in the background (detached mode)?**
- **Answer**: Use the `-d` flag to run the container in detached mode.

**Example**:
```bash
docker run -d my_image
```

---

**68. What is the difference between `docker exec` and `docker attach`?**
- **Answer**: 
  - `docker exec` runs a command in a running container.
  - `docker attach` attaches to the running container's main process.
  
**Example**:
```bash
docker exec -it my_container bash
docker attach my_container
```

---

**69. How do you run a Docker container with a custom hostname?**
- **Answer**: Use the `--hostname` flag to set a custom hostname for the container.

**Example**:
```bash
docker run --hostname mycontainer my_image
```

---

**70. What is the purpose of Docker's `.dockerignore` file?**
- **Answer**: The `.dockerignore` file tells Docker which files to exclude from the build context. It's similar to `.gitignore` and helps to avoid copying unnecessary files into the Docker image.

---

**71. How do you check the status of a running container?**
- **Answer**: You can use the `docker ps` command to list all running containers.

---

**72. How do you view the logs of a Docker container?**
- **Answer**: Use `docker logs` to view logs of a running container.

**Example**:
```bash
docker logs my_container
```

---

**73. How do you stop a running Docker container?**
- **Answer**: Use the `docker stop` command to stop a container.

**Example**:
```bash
docker stop my_container
```

---

**74. What is the difference between `docker pull` and `docker build`?**
- **Answer**:
  - `docker pull` pulls an image from a registry.
  - `docker build` builds an image from a Dockerfile.
  
---

**75. How do you remove a Docker container?**
- **Answer**: You can remove a stopped container using `docker rm`.

**Example**:
```bash
docker rm my_container
```

---

**76. How do you check the resource usage of Docker containers?**
- **Answer**: Use `docker stats` to check real-time resource usage for all containers.

---

**77. What is the purpose of `docker network`?**
- **Answer**: `docker network` is used to manage Docker networks, allowing containers to communicate with each other.

---

**78. What is the default network mode in Docker?**
- **Answer**: The default network mode is `bridge`, which creates a private network for containers to communicate with each other.

---

**79. How do you create a custom Docker network?**
- **Answer**: You can create a custom network using the `docker network create` command.

**Example**:
```bash
docker network create my_custom_network
```

---

**80. How do you map a Docker container port to a host port?**
- **Answer**: You use the `-p` flag to map container ports to host ports.

**Example**:
```bash
docker run -p 8080:80 my_image
```

---

**81. How do you set up Docker logging drivers?**
- **Answer**: Docker supports various logging drivers such as `json-file`, `syslog`, and `fluentd`. You can specify the logging driver in the Docker run command or in the Docker daemon configuration.

---

**82. What is the purpose of `docker build`?**
- **Answer**: `docker build` is used to create an image from a Dockerfile.

---

**83. How do you update an already running Docker container?**
- **Answer**: To update a container, you need to stop it, remove it, and create a new one with the updated image.

---

**84. What is a Docker Swarm?**
- **Answer**: Docker Swarm is Docker's native clustering and orchestration tool, used to manage a group of Docker hosts.

---

**85. How do you manage persistent data in Docker containers?**
- **Answer**: You can use **volumes** or **bind mounts** to manage persistent data.

---

**86. How do you ensure that a Docker container uses the latest image version?**
- **Answer**: Use the `docker pull` command before running the container to ensure the latest image is used.

---

**87. How do you create a Docker container from a public image?**
- **Answer**: You can pull a public image from Docker Hub and create a container from it.

**Example**:
```bash
docker run -d nginx
```

---

**88. What are Docker Daemon and Docker CLI?**
- **Answer**:
  - **Docker Daemon**: It manages Docker containers, images, and networks.
  - **Docker CLI**: It allows users to interact with the Docker Daemon via command-line commands.

---

**89. How do you remove an image in Docker?**
- **Answer**: You can remove an image using `docker rmi`.

**Example**:
```bash
docker rmi my_image
```

---

**90. How do you create a Docker network with a specific driver?**
- **Answer**: Use the `--driver` option to create a custom network with a specific driver.

**Example**:
```bash
docker network create --driver bridge my_network
```

---

**91. What is the `docker inspect` command used for?**
- **Answer**: `docker inspect` is used to retrieve detailed information about a container, image, or volume.

---

**92. How do you configure automatic cleanup of unused Docker objects?**
- **Answer**: Use `docker system prune` to automatically clean up unused images, containers, networks, and volumes.

---

**93. How do you create a Docker container with environment variables?**
- **Answer**: You can pass environment variables using the `-e` flag in `docker run`.

**Example**:
```bash
docker run -e MY_ENV=production my_image
```

---

**94. What is a Docker Compose file?**
- **Answer**: A `docker-compose.yml` file defines services, networks, and volumes for a multi-container Docker application.

---

**95. How do you run multiple containers using Docker Compose?**
- **Answer**: Use `docker-compose up` to start all services defined in the `docker-compose.yml` file.

---

**96. What is Docker's `build context`?**
- **Answer**: Build context refers to the directory where the Dockerfile is located and all files that are accessible to the Docker build process.

---

**97. How do you troubleshoot a Docker container that isn't starting?**
- **Answer**: Check logs using `docker logs`, inspect the container using `docker inspect`, and ensure proper resource allocation and image configuration.

---

**98. How do you integrate Docker with CI/CD pipelines?**
- **Answer**: Docker can be integrated into CI/CD pipelines using tools like Jenkins, GitLab CI, or CircleCI for building, testing, and deploying Docker images.

---

**99. How do you pass a file from your local machine to a Docker container?**
-

 **Answer**: Use the `docker cp` command to copy files from the host to the container.

**Example**:
```bash
docker cp myfile.txt my_container:/path/to/destination
```

---

**100. How do you share data between containers in Docker?**
- **Answer**: You can use Docker volumes or shared networks to share data between containers.

---