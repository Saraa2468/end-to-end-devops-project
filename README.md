Digital Art Store: End-to-End DevSecOps Pipeline
This project demonstrates a production-grade DevSecOps pipeline for a Digital Art Store application. The focus was on building a secure, automated, and observable infrastructure while optimizing resources for a low-spec environment.

🏗️ Architecture Overview
Below is the system architecture showing the flow from code commit to production monitoring.

<img width="3863" height="841" alt="mermaid-diagram-2026-04-23-163011" src="https://github.com/user-attachments/assets/17ba3a47-9aad-4b99-84a0-6553777dc55f" />


🚀 Key Features & Optimization
. Continuous Integration (CI): Fully automated pipeline using Jenkins for building and pushing Docker images.

. Security (Shift-Left): Integrated Trivy vulnerability scanning. The pipeline fails automatically if high-severity vulnerabilities are detected.

. Resource Tuning (The t2.micro Challenge): * Implemented 2GB Linux Swap Space to maintain system stability on limited 1GB RAM.

. Automated Docker Image Cleanup in the post-build stage to prevent disk saturation.

. Infrastructure as Code (IaC): Provisioned the entire AWS environment using Terraform.

. Observability: Real-time monitoring of cluster health and application metrics using Prometheus & Grafana.

🛠️ Tech Stack
. Cloud: AWS (EC2, VPC, Security Groups)

. CI/CD: Jenkins, Docker

. Orchestration: K3s (Lightweight Kubernetes)

. Security: Trivy

. IaC: Terraform

. Monitoring: Prometheus & Grafana

📊 Monitoring in Action
Below are screenshots of the Grafana dashboard showing the system health and memory optimization:
<img width="1742" height="762" alt="Screenshot 2026-04-22 001146" src="https://github.com/user-attachments/assets/1e55dfa6-75af-4fba-98e3-b2978c292f79" />
<img width="1880" height="889" alt="Screenshot 2026-04-22 001230" src="https://github.com/user-attachments/assets/62880573-ac74-4c10-88cf-5fec9c7859db" />

📖 Lessons Learned
Working with a t2.micro instance taught me the importance of efficient resource management. By monitoring CPU and Memory spikes during builds, I was able to fine-tune the system to handle a full Kubernetes stack and a CI/CD tool simultaneously without crashing.



