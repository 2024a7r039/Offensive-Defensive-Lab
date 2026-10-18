# Experiment 6 — DVWA Website Risk Assessment

**Student:** Harleen  
**Course:** Ethical Hacking & Network Security Lab, Model Institute of Engineering and Technology (MIET), Jammu  
**Lab run represented by this report:** Yaseen's run on 18 October 2026, shared with permission  
**Scanner:** OWASP ZAP 2.17.0  
**Target application:** DVWA 1.0.7 on Metasploitable 2 (`192.168.1.100`)  
**Virtualization:** VirtualBox; Kali Linux used as the scanning system

> **Evidence and authorship:** The workflow, screenshots, and scan data in this README are from Yaseen's lab run and are shared for this submission with the instructor's permission. They should not be represented as a scan performed on Harleen's own machine.

## Overview

The purpose of this experiment was to use OWASP ZAP to discover the exposed pages of DVWA and assess the application for potential security weaknesses. The scan was conducted in a virtual practice environment against the intentionally vulnerable target at `http://192.168.1.100/dvwa`.

## Lab arrangement and scope

| Component | Function in this exercise |
|---|---|
| Kali Linux | Runs OWASP ZAP and the browser used to explore DVWA |
| Metasploitable 2 | Hosts the deliberately vulnerable DVWA target |
| OWASP ZAP | Crawls the application, analyzes observed requests, runs active tests, and creates a report |

The assignment handout recommends host-only networking on `192.168.56.x`. In the documented run, the target address used was `192.168.1.100`. Since Metasploitable is intentionally insecure, it should remain within an isolated, authorized lab network and be powered off after use.

## Workflow log

### A. Reachability and application setup

Yaseen's notes state that Kali first checked connectivity to `192.168.1.100` with:

```bash
ping -c 4 192.168.1.100
```

The notes record four replies and zero packet loss. DVWA was then accessed at `/dvwa/login.php` and its security setting was changed to **Low**. The IP, ping, and DVWA security screenshots referenced by the Claude draft were not included in the image files available for this package.

### B. Start ZAP

The source notes record an initial package mirror connection problem, followed by a successful install after changing mirrors. The commands recorded for installing and starting the scanner were:

```bash
sudo apt update
sudo apt install zaproxy -y
zaproxy
```

The available evidence captures the launch, first screen, and workspace. [ZAP launch](113610.png) · [Welcome screen](113755.png) · [Main workspace](114038.png)

### C. Browse DVWA while authenticated

Because DVWA's pages are behind a login, the lab workflow used ZAP's Manual Explore feature to open the application in its launched browser, sign in, and browse the site. Capturing those requests helps the scanner learn about pages that would not be discovered by requesting only the public login page. The available Sites tree screenshot records DVWA routes and request methods, including login, security, and vulnerability endpoints.

![DVWA routes and requests recorded in ZAP](114045.png)

### D. Discover pages with Spider

The Spider was configured with `http://192.168.1.100/dvwa` as the starting URL and recursive crawling enabled. Spider traffic can also be inspected by ZAP's passive scanner. The screenshot shows the Spider configuration dialog.

![Spider scan configuration](114434.png)

### E. Test the target with Active Scan

An Active Scan was configured for the same DVWA target with ZAP's Default Policy and recursion enabled. Active scanning issues test requests that can affect an application, so this stage belongs only on the authorized, intentionally vulnerable lab target.

![Active Scan configuration](114454.png)

### F. Review the output and report

The ZAP Alerts view and generated HTML report were used to review the scan. The available captures show the alert list, alert totals by risk and confidence, and the report's opening/contents page.

![ZAP alert list](120626.png)

![Report alert totals by risk and confidence](121001.png)

![Generated report cover and contents](121020.png)

## Findings summary

### Alert counts

The supplied report data gives the following totals:

| Risk category | Number of alerts |
|---|---:|
| High | 2 |
| Medium | 6 |
| Low | 9 |
| Informational | 6 |
| **All categories** | **23** |

An alert count is a summary of scanner output. Each alert needs review to determine whether it is valid, relevant, and exploitable in context.

### Named alerts and category notes

These names and risk levels are transcribed from the provided Yaseen README. The OWASP category column is a suggested mapping to OWASP Top 10 (2021), not a claim that the scanner independently assigned each category. The actual ZAP alert tags and detail in the exported report should take precedence.

| Alert recorded | Risk | Suggested OWASP Top 10 (2021) mapping |
|---|---|---|
| Remote Code Execution (CVE-2012-1823) | High | A06 — Vulnerable and Outdated Components |
| Source Code Disclosure (CVE-2012-1823) | High | A06 — Vulnerable and Outdated Components |
| Absence of Anti-CSRF Tokens | Medium | A01 — Broken Access Control |
| Application Error Disclosure | Medium | A05 — Security Misconfiguration |
| Content Security Policy (CSP) Header Not Set | Medium | A05 — Security Misconfiguration |
| Directory Browsing | Medium | A05 — Security Misconfiguration |
| HTTP Only Site (plain HTTP) | Medium | A02 — Cryptographic Failures |
| Missing Anti-clickjacking Header | Medium | A05 — Security Misconfiguration |
| Cookie No HttpOnly Flag | Low | A05 — Security Misconfiguration |
| Cookie without SameSite Attribute | Low | A01 — Broken Access Control |
| Server Leaks Information via `X-Powered-By` | Low | A05 — Security Misconfiguration |
| Server Leaks Version Information via `Server` header | Low | A05 — Security Misconfiguration |
| `X-Content-Type-Options` Header Missing | Low | A05 — Security Misconfiguration |

The other Low alerts named in the supplied README are In Page Banner Information Leak, Private IP Disclosure, Timestamp Disclosure (Unix), and a second Application Error Disclosure. The named Informational alerts are Authentication Request Identified, Cookie Poisoning, Information Disclosure (Suspicious Comments), Modern Web Application, and Session Management Response Identified. The provided summary does not enumerate every alert name; the complete exported report should be used for a definitive list.

## What the results indicate

- The two High alerts refer to CVE-2012-1823 and the old PHP-CGI component on Metasploitable. The source README says these were detected but not exploited.
- The Medium alerts call attention to missing CSRF defenses, directory listing, error disclosure, absent security headers, and plain HTTP transport.
- The Low alerts include cookie attribute omissions and server/application version disclosure.
- The scan report described in the source notes did not include SQL Injection or Reflected XSS alerts, although those are expected areas in DVWA. They are not listed here as detected vulnerabilities. Scanner coverage can be incomplete, particularly when authenticated pages or specific inputs are not exercised.

## Takeaway

This run shows how ZAP can first build a picture of an authenticated web application and then produce findings for review. Spidering, browsing while signed in, active testing, and checking the final report each contributed to the assessment. The alerts should be manually verified before remediation decisions are made. In production, relevant confirmed issues would be addressed through patching, safer application and server configuration, secure transport, and removal of unnecessary exposure.

