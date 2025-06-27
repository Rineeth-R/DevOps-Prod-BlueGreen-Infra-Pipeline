**Orchestrating a Blue-Green Deployment Strategy on Kubernetes (AWS EKS) with Jenkins and DevSecOps Tools**

**Project Overview**
Designed, implemented, and managed a fully automated DevSecOps CI/CD pipeline for a Java-based application on Amazon Web Services (AWS) using a blue-green deployment strategy. 
This robust pipeline orchestrates the entire software delivery lifecycle, from code commit to deployment, ensuring zero downtime, easy rollbacks, and enhanced security through a 
"shift-left" approach. The project uses containerization, infrastructure as code, and continuous integration/continuous delivery (CI/CD) to deliver a scalable and resilient 
application on a production-grade Kubernetes cluster.

**Key Responsibilities & Technical Implementations**

**1.	Cloud Infrastructure Provisioning & Containerization (AWS EKS & Docker)**

- Provisioned a dedicated AWS EC2 instance to serve as the Jenkins controller, and set up Docker for containerization tasks.
- Configured a highly available AWS EKS cluster from scratch using EKCTL, ensuring a robust environment for application deployment.
- Automated the process of building the application into a Docker container and pushing the image to a Docker Hub repository.
- Utilized Kubernetes for orchestrating the deployment and management of containerized application pods within the EKS cluster.

**2. Automated CI/CD Pipeline Orchestration (Jenkins)**
- Developed a declarative Jenkins Pipeline script (Jenkinsfile) to automate the end-to-end DevSecOps workflow. The pipeline includes stages for:
-	Code Checkout: Retrieving source code from a GitHub repository.
-	Build & Analysis: Compiling the application with Maven, performing static code analysis with SonarQube, and enforcing quality gates.
-	Artifact Management: Publishing application artifacts to a Nexus repository.
-	Container Security: Performing comprehensive container image scanning with Trivy to detect vulnerabilities.
-	Image Build & Push: Building and pushing tagged Docker images to Docker Hub.
-	Kubernetes Deployment: Deploying the application to the EKS cluster using kubectl.
-	Traffic Switching: Dynamically switching user traffic between the blue and green environments.
-	Configured Jenkins with necessary tools like JDK 17, Node.js 16, and Docker, and established secure authentication to Docker Hub and other tool servers using Jenkins credentials.

**3.	Integrated Security & Quality Implementation (Shift-Left Approach)**
-	Integrated SonarQube into the Jenkins pipeline to perform static code analysis, identifying bugs, vulnerabilities, and code smells at an early stage.
-	Enforced code quality and security standards by implementing a SonarQube Quality Gate within the Jenkins pipeline.
-	Integrated Trivy for comprehensive file system and container image scanning to proactively detect vulnerabilities before deployment.

**4.	Kubernetes Deployment & Scaling**
-	Configured Kubernetes deployments and services within the EKS cluster to ensure high availability and accessibility of the application.
-	Implemented a blue-green deployment strategy to enable zero-downtime application upgrades.
-	Used Kubernetes services (ClusterIP for database and LoadBalancer for the application) to manage internal and external traffic flow.
-	Maintained the previous version of the application in the cluster to enable instant rollbacks in case of issues with the new version.

**5.	Automated Notification & Alerts**
-	Configured Jenkins Extended Email Notification to provide real-time updates on pipeline status (success or failure).
-	Automated the attachment of security scan reports (Trivy) and build logs to email notifications for detailed post-build analysis.

**Key Achievements**
- Successfully designed and implemented a fully automated DevSecOps CI/CD pipeline for a production-level application on AWS EKS, reducing manual effort and improving delivery speed.
-	Achieved robust "shift-left" security by integrating SonarQube and Trivy into early pipeline stages, proactively identifying and mitigating vulnerabilities.
-	Ensured scalable and resilient application deployment by using AWS EKS and a blue-green deployment strategy, providing a zero-downtime upgrade experience.
-	Streamlined the software delivery lifecycle by automating the build, security analysis, and deployment processes, significantly improving efficiency.
-	Provided comprehensive visibility into code quality, security posture, and build status through integrated tooling and automated reporting.

**Technical Stack & Tools**
-	Cloud Platforms: AWS (EC2, EKS).
-	CI/CD: Jenkins, Git, GitHub.
-	Containerization: Docker, DockerHub, Kubernetes.
-	Artifact Management: Nexus.
-	Infrastructure as Code: Terraform.
-	Security Tools: SonarQube, Trivy.
-	Programming & Scripting: Shell Scripting, Java, Node.js.
-	Runtime Environment: JDK 17, Node.js 16.


**JENKINS:**

![Screenshot 2025-06-27 134855](https://github.com/user-attachments/assets/b25b286a-08ff-4acd-be17-f4b0012fbd3b)
![Screenshot 2025-06-27 134617](https://github.com/user-attachments/assets/58e248fd-5799-4f4b-abfe-dc40f16fda21)


**SonarQube**
![Screenshot 2025-06-27 143623](https://github.com/user-attachments/assets/99056829-e685-4789-a3cc-944b8df0b1b6)

**Argo CD:**

![Screenshot 2025-06-27 143536](https://github.com/user-attachments/assets/9f1cfd9d-2ad2-480b-8041-ed751dceae7f)


**Project Output:**
![Screenshot 2025-06-27 135053](https://github.com/user-attachments/assets/a2d29ba7-b518-409b-b04d-084dd3b3c797)

![Screenshot 2025-06-27 135108](https://github.com/user-attachments/assets/b5ef3a88-7d4f-410a-a2fa-c543dec1eeec)

![Screenshot 2025-06-27 141738](https://github.com/user-attachments/assets/2c26ae56-4dd8-4121-bb08-0add842631be)

![Screenshot 2025-06-27 140522](https://github.com/user-attachments/assets/0a2e2acf-94a4-4c7e-8af4-abca30fc2e10)

**MYSQL Database**

![Screenshot 2025-06-27 141750](https://github.com/user-attachments/assets/adb2b3a9-abe3-4f73-95c3-fe27a7ecb40c)


