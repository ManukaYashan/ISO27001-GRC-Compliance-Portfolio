# ISO/IEC 27001:2022 & GRC Case Study: TalentBridge Lanka (Simulated)

**Author:** Manuka Yashan Wijerathne  
**Focus Areas:** Information Security Governance, Risk Management (ISO 27001 / NIST), Policy Review, SOP Documentation & Internal Audit Evidence  

---

## 1. Business Context & Scenario
**TalentBridge Lanka (Pvt) Ltd** is a simulated 35-person recruitment and HR-tech agency operating in Sri Lanka on a hybrid work model. The company operates a custom web-based **Applicant Tracking System (ATS)** and **Employer Management System (EMS)** hosted on AWS (EC2/S3) with Microsoft 365 / Entra ID for corporate identity, holding **45,000+ candidate CVs, NIC numbers, and passport records**.

### The Business Trigger
To secure a high-value recruitment contract with a tier-1 Sri Lankan commercial bank, TalentBridge must pass a client vendor security assessment and demonstrate alignment with **ISO/IEC 27001:2022** and the **Sri Lanka Personal Data Protection Act (PDPA)** within 90 days—while operating under SME budget constraints (1 IT Administrator, no dedicated 24/7 SOC).

---

## 2. Portfolio Artifacts in This Repository

| File / Artifact | Description | Key GRC Skills Demonstrated |
| :--- | :--- | :--- |
| 📊 **[`01_TalentBridge_Risk_Register.csv`](./01_TalentBridge_Risk_Register.csv)** | End-to-end cybersecurity risk register evaluating 6 business risk scenarios (`If... alongside... then...`), inherent vs. residual risk ratings, ISO 27001 Annex A mappings, and risk ownership. | Risk Identification, Qualitative Scoring, Control Mapping, Risk Treatment |
| 📑 **[`02_Policy_Review_and_Audit_Findings.md`](./02_Policy_Review_and_Audit_Findings.md)** | Critique of legacy security policy, updated **Access & Candidate Data SOP (4-Hour Offboarding Rule)**, and 2 formal internal audit findings (*Condition, Criteria, Risk, Recommendation*). | Policy Review, SOP Writing, Gap Analysis, Audit Communication |

---

## 3. Executive Risk Summary & Key Governance Decisions

Rather than applying expensive enterprise tools that do not fit a 35-person business, this assessment focused on practical, high-impact controls:

1. **Eliminating High-Probability Data Leak Channels (Mitigate):**
   * *Issue:* Recruiters were receiving candidate NICs and CVs over personal WhatsApp and using shared intern logins without MFA.
   * *Decision:* Enforced existing Microsoft Entra ID MFA across all 35 staff at zero additional license cost, prohibited shared accounts, and mandated direct candidate uploads via the ATS portal (**ISO 27001 A.8.5 & A.8.12**).
2. **Closing the Offboarding Gap (Mitigate):**
   * *Issue:* 5–7 day delays between HR resignations and IT account deactivation risked departing recruiters exporting client and candidate databases.
   * *Decision:* Implemented a mandatory **4-Hour Offboarding SOP** between HR and IT requiring timestamped revocation evidence (**ISO 27001 A.5.18**).
3. **Pragmatic Risk Acceptance Under Budget Constraints (Accept):**
   * *Issue:* Lack of a 24/7 Security Operations Center (SOC) or commercial SIEM tool (`RSK-06`).
   * *Decision:* Purchasing an enterprise SIEM was financially disproportionate for a 35-person agency. Management formally **Accepted** the residual after-hours monitoring risk, implementing free AWS CloudWatch root/admin login email alerts as a compensating control (**ISO 27001 A.8.16**).

---

## 4. ISO/IEC 27001:2022 Mini Control & Evidence Tracker

| ISO 27001:2022 Control | Control Area | Current Status | Audit Evidence Required |
| :--- | :--- | :--- | :--- |
| **A.5.15 & A.5.16** | Access Control & Identity Management | **Remediated** | Exported ATS user list showing zero shared accounts and RBAC role assignments. |
| **A.5.18** | Access Rights (Offboarding) | **In Progress** | Completed HR-IT Offboarding Tickets showing account deactivation within 4 hours. |
| **A.8.5** | Secure Authentication (MFA) | **Remediated** | Microsoft Entra ID Conditional Access policy screenshot showing 100% MFA enforcement. |
| **A.8.9 & A.8.10** | Cloud Config & Data Deletion | **Planned (30 Days)** | AWS S3 configuration screenshot showing Block Public Access, AES-256, and 24-month lifecycle rule. |
| **A.8.13** | Information Backup Testing | **Scheduled** | Quarterly staging restore test log and recovery time screenshot. |
