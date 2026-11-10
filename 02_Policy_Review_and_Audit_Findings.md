# TalentBridge Lanka (Pvt) Ltd – Policy Review, SOP & Internal Audit Findings
**Document Reference:** GRC-AUD-2026-01  
**Prepared By:** Manuka Yashan Wijerathne (Junior GRC & IT Compliance Analyst - Simulated Case)  
**Frameworks Referenced:** ISO/IEC 27001:2022 (Annex A) & Sri Lanka Personal Data Protection Act (PDPA) No. 9 of 2022

---

## Part 1: Policy Review (Existing Legacy Policy Assessment)

### 1.1 Legacy Policy Excerpt Reviewed
> *"All TalentBridge staff must use strong passwords on the ATS portal, keep candidate CVs confidential, and inform the IT Admin when a staff member resigns so their account can be removed."* (Version 1.0 – 2023)

### 1.2 Governance Review & Identified Gaps
* **What Works Well:** Establishes basic management intent regarding confidentiality and account removal upon termination.
* **Critical Gaps Identified:**
  1. **Ambiguous Authentication Rules:** "Strong passwords" is undefined, and Multi-Factor Authentication (MFA) is not mandated, resulting in interns sharing generic accounts (`intern@talentbridge.lk`).
  2. **No Unstructured Data / WhatsApp Controls:** Does not prohibit recruiters from receiving candidate NICs, passports, and CVs via personal WhatsApp or storing them in local laptop `Downloads` folders.
  3. **No Offboarding SLA:** "Inform the IT Admin" lacks a strict timeframe, leading to 5–7 day delays where former recruiters retain active access to candidate pools.
  4. **No Data Retention Schedule:** Fails to specify how long candidate CVs are stored in AWS S3, conflicting with Sri Lanka PDPA storage limitation principles.

---

## Part 2: Updated Standard Operating Procedure (SOP) – Access & Candidate Data Governance
**SOP ID:** SOP-SEC-01 | **Effective Date:** November 2026 | **Owner:** IT Administrator & HR Manager

### 2.1 User Onboarding & Role-Based Access Control (ISO 27001 A.5.15, A.8.5)
1. **Unique Identities:** Every permanent employee and intern must be issued an individual Microsoft 365 / Entra ID account. Shared accounts are strictly prohibited.
2. **Least Privilege Assignment:** Access to the Applicant Tracking System (ATS) and Employer Management System (EMS) must be assigned using strict RBAC profiles:
   * `Recruiter_Standard`: View/upload candidate CVs for assigned job requisitions only (bulk CSV export disabled).
   * `HR_Coordinator`: Access internal leave and employee records on EMS.
   * `Sys_Admin`: Restricted to 2 authorized technical personnel.
3. **Mandatory MFA:** Users must enroll in Microsoft Authenticator MFA upon first login; password minimum length is set to 12 characters.

### 2.2 Employee Offboarding – The 4-Hour Rule (ISO 27001 A.5.18, A.6.5)
1. **Immediate Trigger:** For voluntary resignations, HR must submit an Offboarding Ticket 24 hours prior to the final working day. For immediate terminations, HR must notify IT via phone/ticket immediately prior to the exit interview.
2. **Access Revocation Checklist (Within 4 Hours):**
   * [ ] Revoke active Microsoft 365 / Entra ID SSO sessions and reset password.
   * [ ] Deactivate ATS and EMS user profile.
   * [ ] Remove user from company WhatsApp/Teams recruitment groups.
3. **Audit Evidence:** IT Admin must attach a timestamped screenshot of the disabled account to the offboarding ticket.

### 2.3 Candidate PII & CV Handling Rules (ISO 27001 A.5.10, A.8.10, A.8.12)
1. **Prohibition of Personal WhatsApp:** Recruiters must not request or receive NIC copies, passport scans, or CVs via personal WhatsApp accounts. Candidates must submit documents directly via the encrypted ATS portal link.
2. **Local Endpoint Hygiene:** Any CV downloaded to a work laptop for formatting/review must be deleted from the local `Downloads` folder every Friday by 5:00 PM.
3. **Automated Cloud Retention:** Candidate records in the AWS S3 bucket with no activity for 24 months will be automatically purged unless the candidate explicitly renews consent.

---

## Part 3: Internal Audit Findings & Mini Gap Assessment

### Finding 01: Shared Intern Accounts and Unenforced MFA on ATS Portal
* **Priority:** High (Critical Risk RSK-01)
* **Condition (What is happening now):** A review of the ATS user table revealed 2 active generic accounts (`intern1@`, `temp_recruiter@`) shared among 4 staff members, and 18 out of 35 total accounts did not have Multi-Factor Authentication (MFA) enabled.
* **Criteria (Standard/Policy):** ISO/IEC 27001:2022 Control A.5.16 (*Identity Management*) and Control A.8.5 (*Secure Authentication*) require uniquely identifiable user accounts and multi-factor authentication for systems storing sensitive PII.
* **Risk (Business Impact):** Compromised credentials cannot be traced to an individual user, and an attacker could exfiltrate 45,000+ candidate records using a single phished password, disqualifying TalentBridge from enterprise banking client contracts.
* **Recommendation:** Disable both shared accounts immediately, issue individual accounts, and enforce a global Entra ID Conditional Access policy requiring MFA for all 35 staff within 7 days.

### Finding 02: Untested Cloud Database Backups
* **Priority:** Medium (Risk RSK-05)
* **Condition (What is happening now):** While automated nightly AWS snapshots are configured for the production ATS database, there is no record or evidence of a test restoration being performed in the past 12 months.
* **Criteria (Standard/Policy):** ISO/IEC 27001:2022 Control A.8.13 (*Information Backup*) requires backup copies to be regularly tested to ensure they meet business recovery objectives.
* **Risk (Business Impact):** During a ransomware attack or database outage, backups may fail to restore properly, breaching the 24-hour recovery SLA with corporate clients.
* **Recommendation:** Schedule a quarterly backup restoration drill to a non-production staging instance and store the completion log and screenshot in the GRC evidence folder.
