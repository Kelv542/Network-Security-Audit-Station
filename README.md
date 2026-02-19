# Network-Security-Audit-Station

# Network Intercept & Web Security Audit Station

## 📋 Project Overview
This project demonstrates the engineering of a mobile security audit station using a Windows tablet as a transparent proxy. By implementing a Man-in-the-Middle (MITM) architecture, I performed deep packet inspection (DPI) and security analysis on encrypted HTTPS traffic originating from a macOS (M1 Pro) endpoint. This lab replicates the setup used by professional security consultants to perform on-site web application audits and network traffic analysis.


🛡️ Technical Implementation
1. Transparent Proxy Engineering

To intercept encrypted traffic from an external device, I configured the Windows tablet to act as a network gateway:

Interface Binding: Reconfigured the Burp Suite proxy listener to bind to 0.0.0.0:8080, bypassing the default loopback restriction to allow external network traffic ingestion.

Certificate Authority (CA) Injection: Executed a "Chain of Trust" modification by exporting the PortSwigger Root CA and manually injecting it into the macOS System Keychain with "Always Trust" settings. This allowed for full TLS termination and plain-text inspection of HTTPS traffic.

2. Traffic Manipulation & Analysis

I performed several manual and automated audit techniques to evaluate endpoint security:

Header Manipulation: Demonstrated identity spoofing by intercepting and modifying User-Agent strings in transit.

Automated Fuzzing (Intruder): Utilized Burp Intruder to perform automated security audits on login endpoints, identifying server-side response discrepancies that could lead to account enumeration.

Sensitive Data Audit: Monitored background HTTP history to identify insecure API calls, clear-text tokens, or PII (Personally Identifiable Information) exposure.

🔍 Security Audit Findings
Category	Observation	Impact	Recommendation
Transport Layer	Successful TLS Termination	High	Implement Certificate Pinning in mobile/desktop apps to prevent MITM.
Authentication	Enumeration via Intruder	Medium	Implement rate-limiting and generic error messages for login failures.
Info Disclosure	Plain-text User-Agent Spoofing	Low	Implement server-side validation for client-provided metadata.


Export to Sheets

🛠️ Tools & Technologies
Analysis Platform: Windows Tablet (Surface/Portable x64)

Target Endpoint: macOS (M1 Pro)

Security Suite: Burp Suite Professional / Community

Protocols: TCP/IP, HTTPS, TLS 1.3, SMB (for cert transfer)

📈 Professional Takeaways
Network Visibility: Developed the ability to analyze encrypted traffic streams that are normally invisible to standard monitoring tools.

Heterogeneous Environment Management: Successfully navigated the interoperability challenges between Windows and macOS security architectures.

Defensive Mindset: Beyond just intercepting traffic, this project allowed me to document how developers can defend against interception using HSTS and Certificate Pinning.

Note: This project was conducted in a controlled lab environment for educational and security auditing purposes only.

## 🏗️ Audit Topology
```mermaid
graph LR
    A[M1 Pro MacBook] -- HTTPS Traffic --> B[Windows Tablet Audit Station]
    B -- Burp Suite Proxy --> C[External Web Servers]
    B -- Analysis & Intercept --> B
    
    style B fill:#f9f,stroke:#333,stroke-width:4px
