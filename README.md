# FUTURE_CS_01 - Vulnerability Assessment Report

## Cyber Security Internship - Task 1: Vulnerability Assessment Report (Future Interns)

---

## 📋 Internship Details

| Property | Details |
|----------|---------|
| **Organization** | Future Interns |
| **Intern** | Sharkjoe Andson Mwalukasa |
| **CIN ID** | FIT/JUL26/CS9735 |
| **Track** | Cyber Security |
| **Task** | 01 - Vulnerability Assessment |
| **Date** | July 18 - July 20, 2026 |
| **Duration** | 3 Days |

---

## 🎯 Overview

This repository contains the comprehensive **Vulnerability Assessment Report** for Task 1 of the Future Interns Cyber Security Internship Program. The assessment was conducted on the **OWASP WebGoat v8.2.2** test application, a deliberately insecure web application designed for educational purposes in web application security.

The primary objective was to identify, classify, and document security vulnerabilities using industry-standard tools and manual testing techniques, mirroring real-world security assessment workflows.

---

## 🛠️ Tools & Technologies Used

| Tool | Purpose | Version |
|------|---------|---------|
| **Nmap** | Network scanning, port enumeration, and service detection | 7.99 |
| **OWASP ZAP** | Automated vulnerability scanning and spidering | 2.17.0 |
| **Mozilla Firefox** | Manual testing and browser developer tools inspection | Latest |
| **Kali Linux** | Primary penetration testing operating system | 2024.x |
| **WebGoat** | Deliberately vulnerable target application | 8.2.2 |

---

## 📊 Key Findings Summary

### Vulnerability Distribution

| Severity Level | Count | Percentage |
|---------------|-------|------------|
| 🔴 **High** | 2 | 16.6% |
| 🟡 **Medium** | 5 | 41.7% |
| 🟢 **Informational** | 5 | 41.7% |
| **Total** | **12** | **100%** |




---

## 🔴 High Severity Vulnerabilities

### 1. SQL Injection (CWE-89)
- **URL:** `http://localhost:8080/WebGoat/register.mvc`
- **Parameter:** `agree`
- **Description:** The application is vulnerable to SQL injection through the 'agree' parameter. User-supplied input is concatenated directly into SQL queries without proper sanitization or parameterization.
- **Impact:** Unauthorized database access, data breach, potential authentication bypass
- **Remediation:** Implement parameterized queries using PreparedStatement, input validation, principle of least privilege

### 2. Spring4Shell - CVE-2022-22965 (CWE-78)
- **URL:** `http://localhost:8080/WebGoat/registration`
- **Parameter:** `class.module.classLoader.DefaultAssertionStatus`
- **Description:** Critical remote code execution vulnerability affecting Spring Framework applications on JDK 9+. Allows attackers to write arbitrary files to the server.
- **Impact:** Complete server compromise, unauthorized code execution, data exfiltration
- **Remediation:** Upgrade Spring Framework to 5.3.18+ or 5.2.20+, implement WAF rules

---

## 🟡 Medium Severity Vulnerabilities

### 3. Absence of Anti-CSRF Tokens (CWE-352)
- **URL:** `http://localhost:8080/WebGoat`
- **Description:** Forms lack Cross-Site Request Forgery protection tokens. Manual inspection confirmed no hidden CSRF token fields in the login form.
- **Remediation:** Implement synchronized token pattern, use Spring Security CSRF protection

### 4. Content Security Policy (CSP) Header Not Set (CWE-693)
- **URL:** `http://localhost:8080/WebGoat`
- **Description:** Missing CSP header increases susceptibility to XSS attacks and data injection.
- **Remediation:** Implement strict CSP header, start with Report-Only mode

### 5. Parameter Tampering (CWE-472)
- **URL:** `http://localhost:8080/WebGoat/register.mvc`
- **Parameter:** `matchingPassword`
- **Description:** Application relies on client-side validation for password confirmation, allowing parameter manipulation.
- **Remediation:** Server-side validation for all input parameters

### 6. Cookie No HttpOnly Flag (CWE-1004)
- **Cookie:** `JSESSIONID`
- **Description:** Session cookie lacks HttpOnly flag, allowing JavaScript access and potential XSS-based session hijacking.
- **Remediation:** Set `HttpOnly=true`, `Secure=true`, `SameSite=Strict` on all session cookies

### 7. Cookie Without SameSite Attribute (CWE-1275)
- **Cookie:** `JSESSIONID`
- **Description:** Cookie set with `SameSite: None`, exposing application to CSRF attacks via cross-site requests.
- **Remediation:** Set `SameSite=Strict` or `SameSite=Lax`

### 8. DOM-Based Cross-Site Scripting - XSS (CWE-79)
- **URL:** `http://localhost:8080/WebGoat/start.mvc#lesson/CrossSiteScripting.lesson/9`
- **Description:** Vulnerable to DOM-Based XSS through improper URL hash fragment handling in Backbone.js routing.
- **Remediation:** Sanitize DOM inputs with DOMPurify, avoid `innerHTML`/`eval()` with user data

---

## 🟢 Informational Findings

| # | Finding | Notes |
|---|---------|-------|
| 9 | Authentication Request Identified | Login form detected — normal behavior |
| 10 | Session Management Response | JSESSIONID cookie present — normal behavior |
| 11 | User Agent Fuzzer | Different responses for different user agents |
| 12 | User Controllable HTML Element | `matchingPassword` field is user-controlled |

---

## 📁 Repository Structure

```
FUTURE_CS_01/
├── README.md                          # This file
├── report/
│   └── Vulnerability_Assessment_Report_WebGoat.pdf
├── screenshots/
│   ├── nmap_basic_scan.png
│   ├── nmap_service_version.png
│   ├── nmap_comprehensive_scan.png
│   ├── zap_alerts_summary.png
│   ├── zap_sql_injection.png
│   ├── zap_spring4shell.png
│   ├── login_form_html.png
│   ├── cookie_details.png
│   ├── xss_console_output.png
│   └── ZAP_Alerts_Summary_PieChart.png
└── raw_data/
    └── nmap_webgoat.txt
```

---

## 📝 Methodology

### Phase 1: Reconnaissance & Network Scanning
- Conducted Nmap scans (basic, service version, comprehensive)
- Performed WHOIS lookup for domain intelligence
- Documented open ports, services, and operating system information

### Phase 2: Automated Vulnerability Scanning
- Configured OWASP ZAP with traditional spider and Firefox browser engine
- Executed active scan against target application
- Documented all alerts with severity classification

### Phase 3: Manual Verification & Testing
- Inspected HTML source code using browser DevTools
- Tested for DOM-Based XSS vulnerabilities
- Analyzed cookie security attributes (HttpOnly, Secure, SameSite)
- Verified automated findings with manual exploitation

---

## 🎯 Remediation Priorities

### Immediate Actions (0-7 Days)
- [ ] Patch Spring Framework to version 5.3.18+ or 5.2.20+
- [ ] Implement parameterized queries for all database interactions
- [ ] Add CSRF tokens to all state-changing forms
- [ ] Configure secure cookie flags (HttpOnly, Secure, SameSite)

### Short-Term Actions (1-4 Weeks)
- [ ] Implement Content Security Policy header
- [ ] Add comprehensive server-side input validation
- [ ] Sanitize DOM inputs using DOMPurify
- [ ] Implement output encoding for XSS prevention

### Long-Term Actions (1-3 Months)
- [ ] Establish regular security assessment schedule
- [ ] Deploy Web Application Firewall (WAF)
- [ ] Develop secure coding standards
- [ ] Implement developer security training program

---

## 📚 References & Resources

| Resource | URL |
|----------|-----|
| OWASP WebGoat | https://owasp.org/www-project-webgoat/ |
| OWASP ZAP | https://www.zaproxy.org/ |
| Nmap | https://nmap.org/ |
| Spring4Shell CVE-2022-22965 | https://spring.io/blog/2022/03/31/spring-framework-rce-early-announcement |
| CWE Top 25 | https://cwe.mitre.org/top25/ |
| OWASP Top 10 | https://owasp.org/www-project-top-ten/ |

---

---

## 📄 License

This report is prepared for educational purposes as part of the Future Interns Cyber Security Internship Program. The vulnerabilities documented herein were identified in a controlled test environment using OWASP WebGoat, a deliberately vulnerable application designed for security education.

**⚠️ Disclaimer:** Do not use the techniques described in this report on systems without explicit authorization. Unauthorized access to computer systems is illegal.

---


*Report Version: 1.0 | Date: July 20, 2026 | CIN: FIT/JUL26/CS9735*
