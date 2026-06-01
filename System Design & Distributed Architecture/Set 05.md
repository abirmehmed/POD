# System Design & Distributed Architecture: Set 5 (Final)
## Cloud-Native Patterns, Edge Computing & Cost-Aware Architecture

## 📘 10 Conceptual Questions
1. **Serverless vs. Containers vs. VMs:** When does each compute model make sense? Compare cold starts, scaling granularity, cost predictability, and operational overhead. When does "serverless first" become a trap vs a superpower?
2. **Edge Computing & CDN Strategies:** What logic belongs at the edge (Cloudflare Workers, Lambda@Edge) vs the origin? Explain cache HIT optimization, image transformation, bot mitigation, and when edge functions create debugging nightmares.
3. **Multi-Region Architecture:** When do you need active-active vs active-passive vs pilot-light disaster recovery? How do you handle data replication, DNS failover, and user session continuity across regions without splitting brains?
4. **Cost-Aware Architecture (FinOps):** How do you optimize cloud spend without sacrificing performance? Explain right-sizing, spot instances, storage lifecycle policies, query budgeting, and how to alert on cost anomalies before the bill arrives.
5. **GitOps & Infrastructure as Code:** How do you manage infrastructure changes with the same rigor as application code? Compare Terraform, Pulumi, and CloudFormation. Explain drift detection, plan/apply workflows, and rollback strategies.
6. **Zero Trust Security Model:** How do you enforce "never trust, always verify" across microservices? Explain mTLS, service identity, short-lived credentials, and how to implement least-privilege access without paralyzing development velocity.
7. **Secret Management at Scale:** How do you rotate API keys, database passwords, and certificates without downtime? Compare Vault, AWS Secrets Manager, and environment variables. Explain secret injection, audit logging, and emergency revocation.
8. **Progressive Delivery & Feature Flags:** How do you decouple deployment from release? Explain canary analysis, automated rollback triggers, user segmentation, and how to enforce flag cleanup to avoid "flag debt".
9. **Observability at Scale:** How do you aggregate metrics, logs, and traces across 100+ services without exploding costs? Explain sampling strategies, metric cardinality limits, log retention policies, and how to debug "unknown unknowns".
10. **Architecture Review & Governance:** How do you prevent architectural drift in fast-moving teams? Explain ADRs, architecture decision forums, automated linting for patterns, and how to balance innovation with consistency.

---

## 🛠️ 10 Practical Tasks

| # | Task | Success Criteria |
|---|------|------------------|
| 1 | **Serverless Function for Image Optimization** | Deploy a Cloudflare Worker or Lambda@Edge that resizes/compresses product images on-the-fly. Verify cache headers, format negotiation (WebP/AVIF), and fallback to origin if transformation fails. |
| 2 | **Multi-Region Failover Simulation** | Configure DNS-based failover (Route53, Cloudflare) between two regions. Simulate region outage, verify traffic reroutes within TTL. Test session continuity and data consistency post-failover. |
| 3 | **Cost Monitoring Dashboard** | Set up cloud cost alerts (AWS Cost Explorer, GCP Billing). Track spend by service, tag, and environment. Configure anomaly detection for sudden spikes. Document optimization opportunities found. |
| 4 | **GitOps Pipeline Setup** | Use Terraform/Pulumi to define infrastructure. Configure GitHub Actions/GitLab CI to `plan` on PR, `apply` on merge to main. Verify drift detection and rollback capability. |
| 5 | **Zero Trust Service-to-Service Auth** | Implement mTLS between two internal services. Use short-lived certificates (SPIFFE/SPIRE or Vault). Verify that requests without valid certs are rejected, even from "trusted" IPs. |
| 6 | **Secret Rotation Without Downtime** | Rotate a database password using a secret manager. Implement dual-read (old + new password) during transition. Verify zero connection failures during rotation. Document rollback steps. |
| 7 | **Progressive Delivery with Canary Analysis** | Deploy a new feature to 5% of users via feature flag. Monitor error rates, latency, and business metrics. Auto-rollback if thresholds breached. Verify 95% of users never see the canary. |
| 8 | **Observability Sampling Strategy** | Configure trace sampling: 100% for errors, 10% for slow requests, 1% for normal traffic. Verify you can still debug issues while reducing observability costs by >80%. |
| 9 | **Architecture Decision Record (ADR) Review** | Pick a past decision in MultiMart. Write a retrospective ADR: what worked, what didn't, what you'd change. Link to related ADRs. Propose one improvement to the decision-making process. |
| 10| **Capstone: Design MultiMart v2** | Draft a one-page architecture for "MultiMart at 10M users". Include: regions, caching layers, async workflows, observability, cost controls, and failure modes. Present trade-offs, not just a diagram. |
