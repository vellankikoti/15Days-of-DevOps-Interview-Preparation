### **Day 2 - Jenkins Hands-On Guide: Mastering Jenkins with 30+ Scenarios and End-to-End Project**

---

**Welcome to the Jenkins Hands-On Guide!** 🎉 Today, we’ll dive deep into Jenkins scenarios, covering a wide range of use cases — from basic configurations to advanced pipelines, cloud integrations, and even an end-to-end CI/CD project that deploys an application on AWS. This guide will help anyone become proficient in Jenkins by following **step-by-step instructions** for each scenario.

By the end of this hands-on guide, you’ll be able to confidently handle Jenkins configurations, troubleshoot real-world issues, and deploy applications end-to-end using Jenkins.

### **Environment Setup for Hands-On Jenkins Scenarios**

For these scenarios, we’ll be using an **AWS EC2 instance** as our Jenkins server. Make sure you have set up your environment as outlined in the previous section. The Jenkins server should be accessible at `http://<EC2-Public-IP>:8080`.

### **Beginner Level: 10 Scenarios to Get Started with Jenkins**

---

1. **Scenario 1: Installing Plugins in Jenkins**

   **Objective**: Install commonly used Jenkins plugins like Git, Docker Pipeline, and Blue Ocean.

   **Steps**:
   1. Go to **Manage Jenkins** > **Manage Plugins**.
   2. Click on **Available** and search for the plugins: `Git`, `Docker Pipeline`, `Blue Ocean`.
   3. Select the plugins and click **Install without restart**.
   4. Go to **Installed** and verify that the plugins are active.

2. **Scenario 2: Creating a Simple Freestyle Job**

   **Objective**: Create a basic Freestyle job that prints a message to the console.

   **Steps**:
   1. Go to **New Item**.
   2. Enter `Simple-Job` as the name and select **Freestyle Project**.
   3. Under **Build** > **Execute Shell**, enter:
      ```bash
      echo "Hello, Jenkins!"
      ```
   4. Click **Save** and **Build Now**.
   5. Check the **Build History** for the output.

3. **Scenario 3: Scheduling a Jenkins Job Using Cron Expressions**

   **Objective**: Schedule a job to run every day at 6:00 AM using Jenkins’ build triggers.

   **Steps**:
   1. Go to **Configure** on the job you created.
   2. Under **Build Triggers**, select **Build periodically**.
   3. Enter the cron expression:
      ```
      H 6 * * *
      ```
   4. Save the configuration.
   5. Verify that the job runs automatically at the specified time.

4. **Scenario 4: Using Git in a Jenkins Freestyle Job**

   **Objective**: Configure a Jenkins job to pull code from a GitHub repository.

   **Steps**:
   1. Go to **Configure** on a new or existing job.
   2. Under **Source Code Management**, select **Git**.
   3. Enter the repository URL: `https://github.com/yourusername/repository.git`.
   4. Save and run the build to check out the code from GitHub.

5. **Scenario 5: Parameterized Jenkins Job**

   **Objective**: Create a job that takes user input (e.g., branch name) before execution.

   **Steps**:
   1. Go to **New Item** and select **Freestyle Project**.
   2. Under **This build is parameterized**, add a **String Parameter** named `BRANCH`.
   3. Under **Source Code Management**, use `$BRANCH` as the branch name.
   4. Save and run the job, providing the branch name as input.

6. **Scenario 6: Backup and Restore Jenkins Configuration**

   **Objective**: Backup Jenkins configurations and restore them to a new Jenkins instance.

   **Steps**:
   1. Go to the Jenkins home directory:
      ```bash
      cd /var/lib/jenkins
      ```
   2. Create a backup:
      ```bash
      tar -czf jenkins_backup.tar.gz .
      ```
   3. To restore, copy the backup file to a new Jenkins instance and extract it in the Jenkins home directory.

7. **Scenario 7: Setting Up Jenkins Job to Poll SCM**

   **Objective**: Automatically build a job whenever there is a new commit in GitHub.

   **Steps**:
   1. Go to **Configure** on the job.
   2. Under **Build Triggers**, select **Poll SCM**.
   3. Enter:
      ```
      H/5 * * * *
      ```
   4. Save and make a new commit in the repository to trigger a build.

8. **Scenario 8: Configuring Email Notifications in Jenkins**

   **Objective**: Configure Jenkins to send email notifications on build status.

   **Steps**:
   1. Install the **Email Extension Plugin**.
   2. Go to **Configure** on the job and add **Post-build Actions** > **Email Notification**.
   3. Enter your email address and SMTP settings.

9. **Scenario 9: Jenkins Pipeline Job Using Declarative Syntax**

   **Objective**: Create a simple pipeline job that builds and deploys a sample project.

   **Steps**:
   1. Go to **New Item** and select **Pipeline**.
   2. Under **Pipeline Definition**, enter:
      ```groovy
      pipeline {
        agent any
        stages {
          stage('Build') {
            steps {
              echo 'Building the project...'
            }
          }
          stage('Test') {
            steps {
              echo 'Running tests...'
            }
          }
          stage('Deploy') {
            steps {
              echo 'Deploying the project...'
            }
          }
        }
      }
      ```
   3. Save and run the pipeline to see the staged execution.

10. **Scenario 10: Jenkins Job to Archive Artifacts**

    **Objective**: Archive build artifacts and make them available for download.

    **Steps**:
    1. Go to **Configure** on the job.
    2. Under **Post-build Actions**, select **Archive the artifacts**.
    3. Specify the files to archive (e.g., `*.jar, *.war`).
    4. Save and build the job.
    5. Verify that the artifacts are available in the build history.

### **Intermediate Level: Jenkins Hands-On Scenarios**

---

#### **Scenario 11: Jenkins Multibranch Pipeline Setup**

**Objective**: Set up a Jenkins Multibranch Pipeline job to automatically build multiple branches in a GitHub repository and detect new branches dynamically.

**Steps**:

1. **Create a GitHub Repository with Multiple Branches**:
   - Create a repository (e.g., `multibranch-pipeline-demo`) and add multiple branches (`main`, `develop`, `feature1`, `feature2`).
   - Add a `Jenkinsfile` to each branch defining the pipeline stages.

2. **Create a Multibranch Pipeline Job in Jenkins**:
   - Go to **New Item** and select **Multibranch Pipeline**.
   - Enter the project name as `Multibranch-Demo`.
   - Under **Branch Sources**, add the GitHub repository URL.
   - Add **credentials** if needed, click **Save**, and then click on **Scan Repository**.

3. **Verify Automatic Branch Detection**:
   - Jenkins will automatically scan for all branches containing a `Jenkinsfile` and create separate jobs for each branch.
   - Go to the Jenkins dashboard and view the builds for each branch.

4. **Create a New Branch in GitHub and Verify**:
   - Create a new branch (`feature3`) in GitHub.
   - Jenkins should automatically detect the new branch and build it.

---

#### **Scenario 12: Setting Up a Jenkins Shared Library**

**Objective**: Use Jenkins Shared Libraries to define reusable pipeline code for multiple projects.

**Steps**:

1. **Create a Shared Library Repository**:
   - Create a new GitHub repository named `jenkins-shared-library`.
   - Inside the repo, create the folder structure: `vars/` and `src/`.
   - Add a file named `vars/common.groovy` with a simple reusable function:
     ```groovy
     def sayHello(name) {
       echo "Hello, ${name}"
     }
     ```

2. **Configure the Shared Library in Jenkins**:
   - Go to **Manage Jenkins** > **Configure System**.
   - Scroll to **Global Pipeline Libraries** and add a new library.
   - Enter the name (e.g., `shared-lib`), select **Modern SCM**, and provide the GitHub repository URL.

3. **Use the Shared Library in a Jenkinsfile**:
   - Create a new project repository and add the following `Jenkinsfile`:
     ```groovy
     @Library('shared-lib') _
     pipeline {
       agent any
       stages {
         stage('Test Shared Library') {
           steps {
             common.sayHello('DevOps Engineer')
           }
         }
       }
     }
     ```

4. **Run the Pipeline**:
   - Create a new **Pipeline Job** in Jenkins and run the pipeline.
   - Verify that the shared library function is called successfully.

---

#### **Scenario 13: Deploying a Dockerized Application Using Jenkins**

**Objective**: Build a Docker image using Jenkins and push it to DockerHub.

**Steps**:

1. **Create a Simple Node.js Application**:
   - Clone the [DevOps-Zero-to-Hero](https://github.com/ruBhutan/DevOps-Zero-to-Hero) repository.
   - Navigate to `docker-app` directory.

2. **Create a Dockerfile**:
   - Create a file named `Dockerfile` with the following content:
     ```Dockerfile
     FROM node:14
     WORKDIR /app
     COPY . .
     RUN npm install
     CMD ["node", "app.js"]
     ```

3. **Create a Jenkins Pipeline for Docker Build**:
   - Use the following `Jenkinsfile`:
     ```groovy
     pipeline {
       agent any
       environment {
         DOCKER_IMAGE = "mydockerrepo/app:latest"
       }
       stages {
         stage('Checkout') {
           steps {
             git 'https://github.com/yourusername/docker-app.git'
           }
         }
         stage('Build Docker Image') {
           steps {
             script {
               docker.build("${DOCKER_IMAGE}")
             }
           }
         }
         stage('Push to DockerHub') {
           steps {
             script {
               docker.withRegistry('https://registry.hub.docker.com', 'dockerhub-credentials') {
                 docker.image("${DOCKER_IMAGE}").push()
               }
             }
           }
         }
       }
     }
     ```

4. **Run the Jenkins Pipeline**:
   - Create a new **Pipeline** job, paste the `Jenkinsfile` script, and build the pipeline.
   - Verify that the Docker image is pushed to DockerHub.

---

#### **Scenario 14: Blue-Green Deployment Using Jenkins Pipelines**

**Objective**: Implement Blue-Green Deployment using Jenkins for zero-downtime releases.

**Steps**:

1. **Set Up Two Separate Kubernetes Environments**:
   - Create two Kubernetes namespaces (`blue` and `green`).

2. **Create Kubernetes YAML Files**:
   - `blue-deployment.yaml`:
     ```yaml
     apiVersion: apps/v1
     kind: Deployment
     metadata:
       name: blue-deployment
       namespace: blue
     spec:
       replicas: 2
       template:
         metadata:
           labels:
             app: myapp
         spec:
           containers:
           - name: app-container
             image: mydockerrepo/app:blue
             ports:
             - containerPort: 8080
     ```

   - `green-deployment.yaml`:
     ```yaml
     apiVersion: apps/v1
     kind: Deployment
     metadata:
       name: green-deployment
       namespace: green
     spec:
       replicas: 2
       template:
         metadata:
           labels:
             app: myapp
         spec:
           containers:
           - name: app-container
             image: mydockerrepo/app:green
             ports:
             - containerPort: 8080
     ```

3. **Create a Jenkins Pipeline for Blue-Green Deployment**:
   ```groovy
   pipeline {
     agent any
     stages {
       stage('Deploy to Blue') {
         steps {
           script {
             sh 'kubectl apply -f blue-deployment.yaml'
           }
         }
       }
       stage('Switch Traffic to Green') {
         steps {
           script {
             sh 'kubectl apply -f green-deployment.yaml'
           }
         }
       }
     }
   }
   ```

4. **Test and Verify the Deployment**:
   - Verify that the application is first deployed to the blue environment and then switches traffic to the green environment.

#### **Scenario 15: Jenkins Integration with Ansible**

**Objective**: Use Jenkins to automate infrastructure configuration and deployments using Ansible.

**Steps**:

1. **Set Up Ansible on the Jenkins Server**:
   - Install Ansible on the Jenkins server:
     ```bash
     sudo yum install ansible -y
     ```

2. **Create an Ansible Playbook**:
   - Create a playbook (`deploy.yaml`) to install Nginx on a remote server:
     ```yaml
     ---
     - hosts: webserver
       become: yes
       tasks:
         - name: Install Nginx
           yum:
             name: nginx
             state: present
     ```

3. **Create a Jenkins Job for Ansible Playbook Execution**:
   - Go to **New Item** and select **Freestyle Project**.
   - Under **Build Environment**, check **Provide Node & Label Parameter Plugin**.
   - Under **Build**, add an **Execute Shell** step with the following command:
     ```bash
     ansible-playbook -i /path/to/inventory deploy.yaml
     ```

4. **Run the Jenkins Job**:
   - Create an **inventory** file listing the remote server IP addresses under `[webserver]`.
   - Run the Jenkins job and verify that Nginx is installed on the target servers.

5. **Verify the Configuration**:
   - Check the remote server by running:
     ```bash
     curl http://<server-ip>
     ```
   - The Nginx welcome page should be displayed.

---

#### **Scenario 16: Using Jenkins with Kubernetes for Deployment**

**Objective**: Use Jenkins to deploy a Dockerized application to a Kubernetes cluster.

**Steps**:

1. **Set Up a Kubernetes Cluster**:
   - Create a Kubernetes cluster using **Minikube** or **AWS EKS**.

2. **Create a Jenkins Pipeline for Kubernetes Deployment**:
   - Create a `Jenkinsfile` for deploying the application:
     ```groovy
     pipeline {
       agent any
       stages {
         stage('Checkout') {
           steps {
             git 'https://github.com/yourusername/app-repo.git'
           }
         }
         stage('Build Docker Image') {
           steps {
             script {
               docker.build("mydockerrepo/app:latest")
             }
           }
         }
         stage('Push to DockerHub') {
           steps {
             script {
               docker.withRegistry('https://registry.hub.docker.com', 'dockerhub-credentials') {
                 docker.image("mydockerrepo/app:latest").push()
               }
             }
           }
         }
         stage('Deploy to Kubernetes') {
           steps {
             script {
               kubernetesDeploy(configs: 'k8s-deployment.yaml', kubeConfig: '<kubeconfig>')
             }
           }
         }
       }
     }
     ```

3. **Create a Kubernetes Deployment YAML File**:
   - `k8s-deployment.yaml`:
     ```yaml
     apiVersion: apps/v1
     kind: Deployment
     metadata:
       name: myapp-deployment
     spec:
       replicas: 3
       selector:
         matchLabels:
           app: myapp
       template:
         metadata:
           labels:
             app: myapp
         spec:
           containers:
           - name: app-container
             image: mydockerrepo/app:latest
             ports:
             - containerPort: 8080
     ```

4. **Run the Jenkins Pipeline**:
   - Jenkins will build the Docker image, push it to DockerHub, and deploy it to the Kubernetes cluster.
   - Verify the deployment using:
     ```bash
     kubectl get pods
     ```

5. **Check the Application**:
   - Access the application using the service IP or Ingress.

---

#### **Scenario 17: Jenkins Backup and Restore Using S3**

**Objective**: Automate Jenkins backup and restore using AWS S3 for disaster recovery.

**Steps**:

1. **Install the AWS CLI on Jenkins**:
   ```bash
   sudo yum install aws-cli -y
   ```

2. **Create an S3 Bucket for Backups**:
   - Go to the AWS Console and create an S3 bucket (e.g., `jenkins-backup-bucket`).

3. **Create a Jenkins Job for S3 Backup**:
   - Go to **New Item** and select **Freestyle Project**.
   - Under **Build**, add a **Execute Shell** step:
     ```bash
     aws s3 sync /var/lib/jenkins s3://jenkins-backup-bucket
     ```

4. **Create a Restore Job in Jenkins**:
   - Go to **New Item** and create another **Freestyle Project**.
   - Under **Build**, add the following command:
     ```bash
     aws s3 sync s3://jenkins-backup-bucket /var/lib/jenkins
     ```

5. **Verify Backup and Restore**:
   - Make changes in Jenkins, run the backup job, delete the changes, and restore using the restore job.

---

#### **Scenario 18: Jenkins Integration with Terraform for IaC**

**Objective**: Use Jenkins to automate infrastructure provisioning using Terraform.

**Steps**:

1. **Install Terraform on the Jenkins Server**:
   ```bash
   sudo yum install -y yum-utils
   sudo yum-config-manager --add-repo https://rpm.releases.hashicorp.com/AmazonLinux/hashicorp.repo
   sudo yum install terraform -y
   ```

2. **Create a Simple Terraform Configuration**:
   - `main.tf`:
     ```hcl
     provider "aws" {
       region = "us-east-1"
     }

     resource "aws_s3_bucket" "jenkins-bucket" {
       bucket = "jenkins-demo-bucket"
       acl    = "private"
     }
     ```

3. **Create a Jenkins Pipeline for Terraform**:
   - Create a `Jenkinsfile`:
     ```groovy
     pipeline {
       agent any
       stages {
         stage('Terraform Init') {
           steps {
             sh 'terraform init'
           }
         }
         stage('Terraform Plan') {
           steps {
             sh 'terraform plan -out=plan.out'
           }
         }
         stage('Terraform Apply') {
           steps {
             sh 'terraform apply plan.out'
           }
         }
       }
     }
     ```

4. **Run the Jenkins Pipeline**:
   - Jenkins will initialize Terraform, create a plan, and apply it to provision the S3 bucket.

5. **Verify the Infrastructure**:
   - Go to the AWS Console and check that the S3 bucket is created.

---

#### **Scenario 19: Running Jenkins on Docker**

**Objective**: Run Jenkins in a Docker container to simplify environment management.

**Steps**:

1. **Install Docker on the Server**:
   ```bash
   sudo yum install docker -y
   sudo service docker start
   ```

2. **Pull the Jenkins Docker Image**:
   ```bash
   docker pull jenkins/jenkins:lts
   ```

3. **Run Jenkins in a Docker Container**:
   ```bash
   docker run -p 8080:8080 -p 50000:50000 --name myjenkins -d jenkins/jenkins:lts
   ```

4. **Access Jenkins**:
   - Go to `http://<Server-IP>:8080` and complete the initial setup.

5. **Persist Jenkins Data**:
   - Create a volume to persist Jenkins data:
     ```bash
     docker run -p 8080:8080 -p 50000:50000 -v jenkins_home:/var/jenkins_home --name myjenkins -d jenkins/jenkins:lts
     ```

6. **Verify Jenkins is Running**:
   - Check the container logs and ensure Jenkins is up and running.

### **Advanced Jenkins Hands-On Scenarios**

---

#### **Scenario 20: Setting Up Jenkins on a Kubernetes Cluster**

**Objective**: Deploy Jenkins on a Kubernetes cluster and configure Jenkins agents for distributed builds.

**Steps**:

1. **Set Up a Kubernetes Cluster**:
   - Use **Minikube**, **K3s**, or a managed service like **AWS EKS** or **GKE**.

2. **Create a Namespace for Jenkins**:
   ```bash
   kubectl create namespace jenkins
   ```

3. **Create a Persistent Volume and Persistent Volume Claim**:
   - `jenkins-volume.yaml`:
     ```yaml
     apiVersion: v1
     kind: PersistentVolume
     metadata:
       name: jenkins-pv
       namespace: jenkins
     spec:
       capacity:
         storage: 5Gi
       accessModes:
         - ReadWriteOnce
       hostPath:
         path: "/data/jenkins-volume/"
     ---
     apiVersion: v1
     kind: PersistentVolumeClaim
     metadata:
       name: jenkins-pvc
       namespace: jenkins
     spec:
       accessModes:
         - ReadWriteOnce
       resources:
         requests:
           storage: 5Gi
     ```

   - Create the PV and PVC:
     ```bash
     kubectl apply -f jenkins-volume.yaml
     ```

4. **Create a Jenkins Deployment YAML File**:
   - `jenkins-deployment.yaml`:
     ```yaml
     apiVersion: apps/v1
     kind: Deployment
     metadata:
       name: jenkins-deployment
       namespace: jenkins
     spec:
       replicas: 1
       selector:
         matchLabels:
           app: jenkins
       template:
         metadata:
           labels:
             app: jenkins
         spec:
           containers:
           - name: jenkins
             image: jenkins/jenkins:lts
             ports:
             - containerPort: 8080
             volumeMounts:
             - name: jenkins-data
               mountPath: /var/jenkins_home
           volumes:
           - name: jenkins-data
             persistentVolumeClaim:
               claimName: jenkins-pvc
     ```

   - Create the Jenkins deployment:
     ```bash
     kubectl apply -f jenkins-deployment.yaml
     ```

5. **Create a Jenkins Service for Access**:
   - `jenkins-service.yaml`:
     ```yaml
     apiVersion: v1
     kind: Service
     metadata:
       name: jenkins-service
       namespace: jenkins
     spec:
       type: NodePort
       ports:
       - port: 8080
         targetPort: 8080
         nodePort: 30000
       selector:
         app: jenkins
     ```

   - Create the service:
     ```bash
     kubectl apply -f jenkins-service.yaml
     ```

6. **Access Jenkins**:
   - Go to `http://<Kubernetes-Node-IP>:30000` to access Jenkins.

7. **Configure Jenkins Kubernetes Plugin**:
   - Go to **Manage Jenkins** > **Configure System**.
   - Add the Kubernetes plugin and configure the Kubernetes cloud settings to allow Jenkins to spin up dynamic agents on the cluster.

8. **Verify Jenkins Builds with Kubernetes Agents**:
   - Create a new pipeline job and configure it to use the `Kubernetes` agent.
   - Run the job and verify that Jenkins dynamically creates a new pod for the build.

---

#### **Scenario 21: Implementing Jenkins as Code Using Job DSL**

**Objective**: Use the Jenkins Job DSL plugin to define jobs as code, making it easier to manage and version control Jenkins jobs.

**Steps**:

1. **Install the Job DSL Plugin**:
   - Go to **Manage Jenkins** > **Manage Plugins** and install the **Job DSL Plugin**.

2. **Create a New Freestyle Job**:
   - Go to **New Item** and create a **Freestyle Project** named `Job-DSL-Example`.
   - Under **Build**, add a **Process Job DSLs** step.
   - Enter the following DSL script:
     ```groovy
     job('example-job') {
       description('This is an example job created using Job DSL')
       scm {
         git('https://github.com/yourusername/your-repo.git')
       }
       triggers {
         scm('H/15 * * * *')
       }
       steps {
         shell('echo "Building the project..."')
       }
     }
     ```

3. **Run the Job**:
   - Run the `Job-DSL-Example` job.
   - Go back to the Jenkins dashboard and verify that a new job named `example-job` has been created.

4. **Verify the Job Configuration**:
   - Go to `example-job` and check the SCM configuration and build steps.
   - Modify the DSL script, re-run the job, and observe changes in the created job.

---

#### **Scenario 22: Jenkins Pipeline for Building and Deploying a Java Web Application**

**Objective**: Create a Jenkins pipeline to build, test, and deploy a Java web application using Maven.

**Steps**:

1. **Set Up a Sample Java Application**:
   - Use the [Spring PetClinic](https://github.com/spring-projects/spring-petclinic) repository as the sample application.

2. **Create a `Jenkinsfile` for the Maven Build**:
   ```groovy
   pipeline {
     agent any
     tools {
       maven 'Maven 3.6.3'
     }
     stages {
       stage('Checkout') {
         steps {
           git 'https://github.com/spring-projects/spring-petclinic.git'
         }
       }
       stage('Build') {
         steps {
           sh 'mvn clean package'
         }
       }
       stage('Test') {
         steps {
           sh 'mvn test'
         }
       }
       stage('Deploy') {
         steps {
           echo 'Deploying application...'
           // Deployment steps can be added here
         }
       }
     }
   }
   ```

3. **Create a New Pipeline Job in Jenkins**:
   - Go to **New Item** and create a **Pipeline Project**.
   - Enter the `Jenkinsfile` content and run the job.

4. **Verify the Build and Test Results**:
   - Check the console output for the `mvn test` results.
   - Validate the created `.war` file in the target directory.

5. **Add Deployment Steps**:
   - Extend the `Jenkinsfile` to include deployment to Tomcat or a cloud provider.

---

#### **Scenario 23: Using Jenkins Groovy Scripting for Custom Pipelines**

**Objective**: Use Groovy scripting to create complex Jenkins pipelines with conditional logic and shared variables.

**Steps**:

1. **Create a Groovy Script in Jenkinsfile**:
   - Use a `Jenkinsfile` with Groovy scripting:
     ```groovy
     def notifySlack(buildStatus) {
       if (buildStatus == 'SUCCESS') {
         echo 'Build was successful'
       } else {
         echo 'Build failed'
       }
     }

     pipeline {
       agent any
       stages {
         stage('Checkout') {
           steps {
             script {
               git 'https://github.com/yourusername/sample-repo.git'
             }
           }
         }
         stage('Build') {
           steps {
             script {
               def buildResult = sh(script: 'mvn clean package', returnStatus: true)
               if (buildResult != 0) {
                 currentBuild.result = 'FAILURE'
               }
             }
           }
         }
       }
       post {
         always {
           script {
             notifySlack(currentBuild.result)
           }
         }
       }
     }
     ```

2. **Run the Jenkins Job**:
   - Verify the build results and custom notification logic.

#### **Scenario 24: Jenkins with AWS CodeBuild and CodeDeploy**

**Objective**: Use Jenkins to trigger AWS CodeBuild for building the application and AWS CodeDeploy for deploying it to EC2 instances.

**Steps**:

1. **Create a Buildspec File for AWS CodeBuild**:
   - Create a `buildspec.yml` file in your project root:
     ```yaml
     version: 0.2
     phases:
       install:
         runtime-versions:
           java: corretto11
       build:
         commands:
           - echo "Building the application"
           - mvn clean package
     artifacts:
       files:
         - target/*.jar
     ```

2. **Create a Jenkins Pipeline with AWS Integration**:
   - Use the following `Jenkinsfile`:
     ```groovy
     pipeline {
       agent any
       stages {
         stage('Trigger CodeBuild') {
           steps {
             script {
               awsCodeBuild projectName: 'codebuild-project-name'
             }
           }
         }
         stage('Deploy to EC2 Using CodeDeploy') {
           steps {
             script {
               awsCodeDeploy applicationName: 'CodeDeployApp',
                 deploymentGroupName: 'CodeDeployGroup',
                 s3Location: 's3://my-bucket/app-latest.zip'
             }
           }
         }
       }
     }
     ```

3. **Verify the Build and Deployment**:
   - Check the AWS CodeBuild console to verify that the build is triggered.
   - Go to the CodeDeploy console to view deployment status.

4. **Test the Application on EC2**:
   - SSH into your EC2 instance and verify that the application is deployed.

---

#### **Scenario 25: Jenkins Integration with AWS ECS and EKS**

**Objective**: Use Jenkins to build a Docker image, push it to Amazon ECR, and deploy the application to AWS ECS and EKS.

**Steps**:

1. **Create an ECR Repository**:
   - Go to the AWS Console and create a repository named `my-app-repo`.

2. **Create a Jenkinsfile for Docker Build and Push**:
   ```groovy
   pipeline {
     agent any
     environment {
       AWS_ACCOUNT_ID = 'your-aws-account-id'
       ECR_REPO_NAME = 'my-app-repo'
       IMAGE_TAG = 'latest'
       REGION = 'us-east-1'
     }
     stages {
       stage('Docker Build') {
         steps {
           script {
             docker.build("${AWS_ACCOUNT_ID}.dkr.ecr.${REGION}.amazonaws.com/${ECR_REPO_NAME}:${IMAGE_TAG}")
           }
         }
       }
       stage('Docker Push') {
         steps {
           script {
             docker.withRegistry("https://${AWS_ACCOUNT_ID}.dkr.ecr.${REGION}.amazonaws.com") {
               docker.image("${AWS_ACCOUNT_ID}.dkr.ecr.${REGION}.amazonaws.com/${ECR_REPO_NAME}:${IMAGE_TAG}").push()
             }
           }
         }
       }
       stage('Deploy to EKS') {
         steps {
           sh 'kubectl apply -f k8s-deployment.yaml'
         }
       }
     }
   }
   ```

3. **Create a Kubernetes Deployment YAML File**:
   - `k8s-deployment.yaml`:
     ```yaml
     apiVersion: apps/v1
     kind: Deployment
     metadata:
       name: myapp-deployment
     spec:
       replicas: 2
       selector:
         matchLabels:
           app: myapp
       template:
         metadata:
           labels:
             app: myapp
         spec:
           containers:
           - name: app-container
             image: ${AWS_ACCOUNT_ID}.dkr.ecr.${REGION}.amazonaws.com/${ECR_REPO_NAME}:${IMAGE_TAG}
             ports:
             - containerPort: 8080
     ```

4. **Run the Jenkins Pipeline**:
   - Jenkins will build the Docker image, push it to ECR, and deploy it to the EKS cluster.

5. **Access the Application**:
   - Use the Kubernetes service or ingress to access the deployed application.

---

#### **Scenario 26: Implementing Jenkins HA (High Availability) Using Kubernetes**

**Objective**: Set up Jenkins in a High Availability (HA) configuration using Kubernetes to ensure fault tolerance and scalability.

**Steps**:

1. **Set Up a Shared File System for Jenkins Home Directory**:
   - Use AWS EFS, GCP Filestore, or NFS as the shared storage for the Jenkins home directory.

2. **Create a Jenkins Master and Agent Deployment**:
   - `jenkins-master-deployment.yaml`:
     ```yaml
     apiVersion: apps/v1
     kind: Deployment
     metadata:
       name: jenkins-master
       namespace: jenkins
     spec:
       replicas: 2
       selector:
         matchLabels:
           app: jenkins
           role: master
       template:
         metadata:
           labels:
             app: jenkins
             role: master
         spec:
           containers:
           - name: jenkins
             image: jenkins/jenkins:lts
             ports:
             - containerPort: 8080
             volumeMounts:
             - name: jenkins-home
               mountPath: /var/jenkins_home
           volumes:
           - name: jenkins-home
             persistentVolumeClaim:
               claimName: jenkins-pvc
     ```

3. **Create a Jenkins Agent Deployment**:
   - `jenkins-agent-deployment.yaml`:
     ```yaml
     apiVersion: apps/v1
     kind: Deployment
     metadata:
       name: jenkins-agent
       namespace: jenkins
     spec:
       replicas: 3
       selector:
         matchLabels:
           app: jenkins
           role: agent
       template:
         metadata:
           labels:
             app: jenkins
             role: agent
         spec:
           containers:
           - name: jnlp
             image: jenkins/inbound-agent:latest
             args:
               - -url
               - "http://jenkins-master.jenkins.svc.cluster.local:8080"
               - ${JENKINS_SECRET}
               - ${JENKINS_NAME}
     ```

4. **Set Up Load Balancer for Jenkins**:
   - Create a service for Jenkins master with type `LoadBalancer`:
     ```yaml
     apiVersion: v1
     kind: Service
     metadata:
       name: jenkins-service
       namespace: jenkins
     spec:
       type: LoadBalancer
       ports:
         - port: 8080
           targetPort: 8080
       selector:
         app: jenkins
         role: master
     ```

5. **Access Jenkins and Verify HA**:
   - Check the load balancer URL and observe Jenkins' high availability configuration.
   - Bring down one Jenkins master pod to verify failover.

---

#### **Scenario 27: Jenkins Parallel and Sequential Stages with Advanced Groovy Scripting**

**Objective**: Use advanced Groovy scripting to implement parallel and sequential stages in a single Jenkins pipeline.

**Steps**:

1. **Create a Jenkinsfile with Parallel and Sequential Stages**:
   ```groovy
   pipeline {
     agent any
     stages {
       stage('Build and Test in Parallel') {
         parallel {
           stage('Build') {
             steps {
               echo 'Building...'
               sh 'mvn clean install'
             }
           }
           stage('Unit Test') {
             steps {
               echo 'Running Unit Tests...'
               sh 'mvn test'
             }
           }
         }
       }
       stage('Integration Testing') {
         steps {
           echo 'Running Integration Tests...'
           sh 'mvn verify'
         }
       }
     }
   }
   ```

2. **Run the Jenkins Pipeline**:
   - Verify that the build and test stages are executed in parallel, while the integration test runs sequentially afterward.

3. **Visualize the Pipeline**:
   - Use Jenkins Blue Ocean to visualize the parallel and sequential stages.

---

### **Day 2 Conclusion**

Congratulations on completing Day 2 of our **15-Day DevOps Interview Preparation Series**! Today, we covered a comprehensive set of Jenkins scenarios, starting from beginner to advanced topics. We explored Jenkins pipelines, integration with Docker, Kubernetes, AWS services, and more complex use cases.

**Key Takeaways**:
1. **Basic Configurations**: Plugin management, backup, scheduling jobs, and freestyle projects.
2. **Intermediate Scenarios**: Multibranch pipelines, Blue-Green deployments, Jenkins shared libraries.
3. **Advanced Use Cases**: AWS integrations, Kubernetes HA, Groovy scripting, and parallel pipelines.

In Day 3, we’ll dive into **Ansible** — exploring scenarios for configuration management, orchestration, and automating cloud environments!

For more DevOps updates, connect with me on [LinkedIn](https://www.linkedin.com/in/vellankikoti/).


