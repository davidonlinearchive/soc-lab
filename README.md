
# **Incident Report: Phishing Attack Leading to Database Credential Compromise**

## **Overview**

An attacker successfully conducted an OSINT reconnaissance against a new employee named **Alex**, the **Database Administrator Intern** at *InsureCo*, a small insurance firm with approximately 50 employees. The attacker used a **phishing email** impersonating the outsourced software development vendor, **AtlasConsult Co.**, to deceive Alex into submitting administrative credentials.

## **Phishing Email Details**

**Subject:** URGENT: Immediate Action Required on Client Database Issue  
**To:** alex@insureco.com  
**From:** Atlas Support <support@atlasconsult.co.com>

---

**Email Body:**

> Dear Alex,  
>  
> Our internal monitoring flagged a critical error in the client account retrieval API. Multiple users have reported missing account data tied to our database backend.  
>  
> We need your assistance reviewing the issue as your credentials are required to access the admin report. Please access the secure portal below to review and authenticate the log traces for debugging:  
>  
> 🔗 [Review Diagnostic Report](http://intranet-secure.co-system.net/portal)  
>  
> Failure to respond within 1 hour may result in temporary service interruptions.  
>  
> Regards,  
> IT Support Team  
> AtlasConsult Co.

---

## **Outcome**

- Alex submitted **valid credentials** on the malicious portal.
- Attacker gained **unauthorized access to Cloud SQL (PostgreSQL)**.
- **Wazuh SIEM** triggered alarms for **unusual data exfiltration**.
- Investigation traced the attacker's **IP address to St. Petersburg, Florida**.

---

## **Technical Stack**

- **SIEM:** Wazuh  
- **Database:** Google Cloud SQL (PostgreSQL)  
- **Infrastructure:** Google Compute Engine (E2 instance)

---

## **Mitigation Strategies**

### 🔒 **1. Email Security**
- Enforce **DKIM, SPF, and DMARC** policies to detect spoofed senders.
- Enable **anti-phishing policies** in the email gateway (e.g., Gmail Advanced Protection).
- Train users with **phishing simulations** (e.g., GoPhish).

### 👤 **2. Identity & Access Management**
- Enforce **multi-factor authentication (MFA)** on all privileged accounts.
- Rotate and revoke credentials immediately after compromise.
- Use **IAM roles** with **least privilege access**.

### 📊 **3. Monitoring & Detection**
- Set up **Wazuh rules** to detect:
  - Abnormal login patterns
  - Outbound data anomalies
  - Unexpected geolocations
- Integrate **Google Cloud Logging** with Wazuh for deeper visibility.

### 🧪 **4. Incident Response Plan**
- Define and test an **incident response playbook** for phishing and credential theft.
- Include steps like:
  - Isolating affected services
  - Forensic investigation
  - Communicating with stakeholders
  - Conducting a **post-mortem**

### 🚫 **5. Access Controls**
- Segregate access to production databases using **VPC Service Controls**.
- Enforce **SSL-only** database connections.
- Use **IAM database authentication** instead of static credentials.

---

## **Conclusion**

This incident highlights the critical need for strong identity protection, vigilant SIEM monitoring, and user security awareness. Had proper MFA and phishing training been in place, the attack could have been prevented or detected much earlier.
