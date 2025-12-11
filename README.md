# zero-trust-azure-architecture
Azure Zero Trust Architecture reference diagram by Aditya Shankar
# Azure Zero Trust Reference Architecture — Aditya Shankar

**Purpose:** Practical, implementable Zero Trust reference architecture for Azure environments, focused on identity-first security, least privilege, and detection-aware controls.

## Key principles
- **Identity-first**: Azure AD + Conditional Access (MFA, device posture, named locations) is the primary control plane.  
- **Least privilege**: PIM + JIT for privileged access; managed identities for workloads.  
- **Segmented network**: Subnet-level NSGs, Azure Firewall for egress, and private endpoints for data services.  
- **Protect the perimeter**: CDN + App Gateway(WAF) + DDoS protection.  
- **Observability**: Centralized logging (Log Analytics) + SIEM for correlation and automated playbooks.  
- **Governance**: Azure Policy and Blueprints to enforce baselines.

## Components
- Identity: Azure AD, Conditional Access, PIM, Managed Identities  
- Perimeter: CDN, Application Gateway (WAF), DDoS Protection  
- Network: VNet, NSGs, Azure Firewall, Private Endpoints  
- Workloads: Web App / AKS, API servers, Private DB, Storage Account  
- Management: Azure Bastion, JIT Jumpbox, Key Vault  
- Monitoring: Log Analytics, SIEM (e.g., Microsoft Sentinel), Automation playbooks  
- Governance: Azure Policy / Blueprints

## Typical flows
1. User authenticates via Azure AD → Conditional Access enforces MFA/device posture → traffic reaches App Gateway (WAF).  
2. WAF forwards to Frontend, which calls APIs in AppTier. APIs use Managed Identities to access Key Vault and DataTier (private endpoints).  
3. All relevant telemetry (Identity, App, DB, WAF, NSG flow) is sent to Log Analytics → SIEM → Automated playbooks for containment and remediation.

## Quick recommendations (practical)
- Enforce CA for all privileged and high-risk apps (no legacy auth).  
- Migrate service principals to managed identities where possible; scope RBAC narrowly.  
- Implement JIT for management ports; use Bastion for secure admin sessions.  
- Ingest identity signals + admin audit logs into SIEM; use behavioral detections for lateral movement.  
- Use Azure Policy to block public storage and enforce private endpoints.

