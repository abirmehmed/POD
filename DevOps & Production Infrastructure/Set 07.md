# DevOps & Production Infrastructure: Set 7
## Security Hardening, Secrets Management & Zero-Trust Infrastructure

## 📘 10 Conceptual Questions
1. **Defense in Depth:** Why is relying solely on a firewall or a WAF insufficient? Explain the layers of security (Network, Host, Container, Application, Data) and how a breach in one layer shouldn't compromise the whole system.
2. **Secrets Management Evolution:** Why are environment variables and `.env` files considered insecure for production? Explain the progression from hardcoded secrets → Env Vars → Cloud Key Management (KMS) → HashiCorp Vault, and the concept of "ephemeral secrets".
3. **Least Privilege & IAM Scoping:** What is the Principle of Least Privilege (PoLP)? Why is attaching `AdministratorAccess` or `*:*` permissions to an EC2 instance profile or CI/CD pipeline a catastrophic risk, and how do you scope roles correctly?
4. **Network Policies & Zero Trust:** In Kubernetes or Docker, why is "default allow" networking dangerous? Explain how Network Policies enforce a Zero Trust model where pods/containers cannot communicate unless explicitly permitted.
5. **Image Scanning & Supply Chain Security:** What is an SBOM (Software Bill of Materials)? Why must you scan Docker images for CVEs (Common Vulnerabilities and Exposures) before deployment, and how do base image choices (Alpine vs. Distroless) affect your attack surface?
6. **TLS Termination & mTLS:** What is the difference between terminating TLS at the Load Balancer (ingress) vs. end-to-end encryption? When and why should internal microservices use mutual TLS (mTLS) to authenticate each other?
7. **CIS Benchmarks:** What are the Center for Internet Security (CIS) benchmarks? Why should you apply them to your host OS, Docker daemon, and Kubernetes cluster, and what is the risk of ignoring them?
8. **Runtime Security & Anomaly Detection:** How do you detect if a container has been compromised and a hacker is spawning a shell? Explain tools like Falco that monitor system calls (syscalls) rather than just network traffic.
9. **Audit Logging vs. Application Logging:** What is the difference between debugging logs and audit trails? Why must infrastructure audit logs (e.g., "Who changed the security group?", "Who accessed the secret?") be immutable and stored separately?
10. **Compliance Frameworks (SOC2, GDPR, HIPAA):** You don't need to be a lawyer, but technically, what do these frameworks demand? Explain concepts like data encryption at rest/transit, right to be forgotten, and separation of duties in infrastructure access.

---

## 🛠️ 10 Practical Tasks

| # | Task | Success Criteria |
|---|------|------------------|
| 1 | **SSH & Host Hardening**<br>Harden a Linux server: Disable root login, disable password authentication (keys only), change default SSH port, install and configure `fail2ban`. Verify you can still log in, but brute-force attempts are blocked. | Root/password login returns "Permission denied". Port 22 is closed/filtered. `fail2ban` bans IP after 3 failed attempts. |
| 2 | **Container Image Scanning**<br>Scan your MultiMart Docker image using a tool like Trivy or Grype. Identify all "Critical" and "High" CVEs. Rebuild the image using a smaller/different base image or updating dependencies to eliminate them. | Trivy report shows 0 Critical/High vulnerabilities. Image size remains optimized. |
| 3 | **Implement a Secrets Manager**<br>Set up a local instance of HashiCorp Vault (or use AWS Secrets Manager free tier). Store a database password. Write a script or app config that fetches the secret dynamically at runtime without hardcoding it. | App starts using the secret fetched from Vault. No passwords exist in `.env`, Git, or plain text on disk. |
| 4 | **Kubernetes Zero-Trust Networking**<br>Apply a `default-deny-all` NetworkPolicy to your namespace. Create a specific policy that *only* allows your PHP pod to talk to the MySQL pod on port 3306, and Redis on 6379. Block all other egress. | `kubectl exec` into PHP pod cannot `curl` external sites or other namespaces. DB and Redis connections still work perfectly. |
| 5 | **Generate an SBOM**<br>Use a tool like Syft to generate a Software Bill of Materials for your MultiMart Docker image. Export it to CycloneDX or SPDX format. Store it as a CI/CD artifact. | `syft multimart:latest -o cyclonedx-json > sbom.json` runs successfully. SBOM lists all OS packages and Composer/NPM dependencies. |
| 6 | **IAM Least Privilege Role**<br>Create an IAM Role/Policy for your CI/CD pipeline. Grant *only* the permissions required to pull/push the specific ECR image and update the specific EKS deployment. Deny all S3 and EC2 permissions. Test by trying to list S3 buckets. | Pipeline works perfectly. Attempting to run `aws s3 ls` with those credentials returns "Access Denied". |
| 7 | **Strict TLS & Security Headers**<br>Configure your Nginx/Ingress to enforce TLS 1.2+ only. Add Strict-Transport-Security (HSTS), X-Content-Type-Options, and a basic Content-Security-Policy (CSP). Test with Mozilla Observatory. | Observatory scores A or A+. HTTP requests redirect to HTTPS. Insecure headers are removed. |
| 8 | **Runtime Security with Falco**<br>Install Falco in your local K8s cluster or Docker host. Trigger an event (e.g., `kubectl exec -it <pod> -- sh` or running `chmod 777`). Check the Falco logs to see the alert. | Falco logs an alert: "Terminal shell in container" or similar. Proves runtime monitoring is active. |
| 9 | **Infrastructure Audit Trails**<br>Configure `auditd` on Linux or CloudTrail on AWS to log all `sudo` commands and IAM role assumptions. Trigger a `sudo` action and locate the exact log entry. | Log clearly shows: Timestamp, User, Command executed, and Source IP. Logs are in a format that can be shipped to a SIEM. |
| 10| **Security Posture Documentation**<br>Create a `SECURITY.md` in your repo. Document your threat model, how secrets are handled, how images are scanned, and the incident response plan for a compromised server. | Document covers the 4 pillars: Prevention, Detection, Response, and Recovery. Clear and actionable for a new team member. |
