# 🛡️ Web Application Firewall using BunkerWeb + ModSecurity

A Docker-based Web Application Firewall (WAF) laboratory that protects the **Damn Vulnerable Web Application (DVWA)** using **BunkerWeb**, **ModSecurity**, and the **OWASP Core Rule Set (CRS)**.

This project demonstrates how a reverse proxy WAF can detect and block common web attacks such as **SQL Injection (SQLi)** and **Cross-Site Scripting (XSS)** before they reach the target application.

---

# Architecture

```
                    Browser
                       │
                       ▼
            +---------------------+
            |     BunkerWeb       |
            |---------------------|
            | • Reverse Proxy     |
            | • ModSecurity WAF   |
            | • OWASP CRS         |
            | • Rate Limiting     |
            +----------+----------+
                       │
                       ▼
            +---------------------+
            |        DVWA         |
            +----------+----------+
                       │
                       ▼
            +---------------------+
            |      MariaDB        |
            +---------------------+
```

---

# Features

- Docker-based deployment
- Reverse Proxy using BunkerWeb
- ModSecurity Web Application Firewall
- OWASP Core Rule Set (CRS)
- SQL Injection protection
- Cross-Site Scripting (XSS) protection
- Docker network isolation
- Security testing using DVWA

---

# Tech Stack

- Docker Compose
- BunkerWeb
- ModSecurity
- OWASP CRS
- DVWA
- MariaDB

---

# Project Structure

```
.
├── docker-compose.yml
├── README.md
├── modsec
│   └── custom-rules.conf
└── screenshots
```

---

# Deployment

Clone repository

```bash
git clone https://github.com/<your-username>/Modsec_With_BunkerWeb.git
cd Modsec_With_BunkerWeb
```

Start containers

```bash
docker compose up -d
```

Verify containers

```bash
docker ps
```

Open DVWA

```
http://localhost
```

---

# Security Testing

## SQL Injection

Payload

```sql
1' OR '1'='1
```

Result

```
HTTP 403 Forbidden
```

The request was blocked by **ModSecurity** before reaching DVWA.

---

## Cross Site Scripting (XSS)

Payload

```html
<script>alert(1)</script>
```

Result

```
HTTP 403 Forbidden
```

---

# Components

## BunkerWeb

Acts as the reverse proxy and Web Application Firewall.

Responsibilities:

- Reverse Proxy
- Request Inspection
- TLS Termination
- Traffic Filtering
- Rate Limiting

---

## ModSecurity

ModSecurity analyzes every incoming HTTP request and compares it against security rules.

If a request matches a malicious signature, it is immediately blocked.

---

## OWASP Core Rule Set

The OWASP CRS provides a comprehensive set of security rules capable of detecting:

- SQL Injection
- Cross Site Scripting (XSS)
- Local File Inclusion (LFI)
- Remote Code Execution (RCE)
- Command Injection
- Protocol Violations
- HTTP Anomaly Detection

---

# Testing Result

| Attack | Payload | Result |
|---------|----------|--------|
| SQL Injection | `1' OR '1'='1` | ✅ Blocked (403) |
| Reflected XSS | `<script>alert(1)</script>` | ✅ Blocked (403) |
| Stored XSS | `<img src=x onerror=alert(1)>` | ✅ Blocked |
| Command Injection | `127.0.0.1; whoami` | ⏳ Planned |
| Local File Inclusion | `../../../../etc/passwd` | ⏳ Planned |

---

# Future Improvements

- Custom ModSecurity Rules
- ModSecurity Audit Logging
- Grafana Dashboard
- ELK Stack Integration
- Automated Security Testing
- HTTPS using Let's Encrypt
- Custom Rule Tuning

---

# Learning Outcomes

This project demonstrates understanding of:

- Reverse Proxy Architecture
- Web Application Firewall (WAF)
- ModSecurity
- OWASP Core Rule Set
- Docker Networking
- Web Security Testing
- SQL Injection Mitigation
- Cross-Site Scripting Mitigation

---

# Screenshots

## SQL Injection Blocked

> *(Insert screenshot here)*

## XSS Blocked

> *(Insert screenshot here)*

## BunkerWeb Dashboard

> *(Insert screenshot here)*

---

# Author

**I Gusti Ngurah Ryo Adi Tarta**

Bachelor of Computer Science

Interested in:

- Network Security
- Cybersecurity
- Infrastructure
- SOC Engineering
