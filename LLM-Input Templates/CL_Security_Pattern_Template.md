# ICL Security Pattern Template (Developer Copy)

A universal, language-agnostic secure coding guidance template for LLM-assisted software development using In-Context Learning (ICL).  
Aligned with **OWASP ASVS, NIST SSDF, Microsoft SDL, CERT Secure Coding, ISO/IEC 27034**.

---

## 2. LLM Role Setup

### Paste this EXACT block at the top of every prompt:

```text
You are a secure coding assistant. Your priorities are, in this exact order:
1. Correctness
2. Security
3. Maintainability
4. Performance

You MUST:
- Follow OWASP ASVS and OWASP Top 10 controls.
- Apply NIST SSDF and Microsoft Secure Coding guidance.
- Avoid known CWE weaknesses whenever safe alternatives exist.
- Use secure defaults and fail securely.
- Clearly separate functional logic from security checks where appropriate.
```

---

## 3. Development Context

Fill this in **before** requesting code from the LLM.

### 3.1 Feature / Module

Examples:  
- Secure login endpoint  
- Audit logger  
- Firmware update verifier  
- Payment API  

**Your Description:**
```text
{Describe what needs to be built}
```

### 3.2 Language & Framework
```text
{Python + FastAPI, C# ASP.NET Core, Java Spring, Node.js Express,
Go Gin, C++ CLI, Rust Actix, etc.}
```

### 3.3 Runtime / Platform
```text
{.NET 8, JVM 17, Python 3.11, AWS Lambda, Azure App Service,
Kubernetes, OT/ICS gateway, etc.}
```

### 3.4 Data Sensitivity
- Public  
- Internal  
- Confidential  
- Secret / Highly regulated  

### 3.5 Threat Profile
- Low – prototypes, non-sensitive data  
- Medium – typical enterprise apps  
- High – auth, payments, PII/PHI, ICS/OT, safety-critical  

### 3.6 Domain
```text
{Web / API / Microservice / Mobile / Desktop / ICS / OT / Cloud-native / CLI}
```

---

## 4. Trust & Threat Model

### 4.1 Trust Boundaries

| Zone | Examples |
|------|----------|
| Untrusted | Internet users, file uploads, headers, external APIs |
| Semi-trusted | partner APIs, MQ, internal dashboards |
| Trusted | DB, identity provider, HSM, secrets vault |

### 4.2 Threat Concerns

- Injection (SQL/NoSQL/Command/Template/LDAP)  
- XSS  
- CSRF / session fixation  
- IDOR  
- SSRF / LFI / RFI  
- Unsafe deserialization  
- File traversal / arbitrary file write  
- Weak crypto / hard-coded secrets  
- Sensitive data leakage  
- Supply chain abuse  
- DoS / resource exhaustion  

---

## 5. Security Objectives

- Confidentiality  
- Integrity  
- Availability  
- Authentication  
- Authorization (RBAC/ABAC, least privilege)  
- Traceability & Auditing  
- Safety / ICS protection  

---

## 6. Functional & Security Specification

### 6.1 Functional Steps

```text
1. Receive request/input
2. Validate schema and reject invalid payloads
3. Execute logic / DB / external calls
4. Return controlled response with safe error handling
```

### 6.2 Security Requirements

- Strict whitelisting validation  
- Parameterized queries / ORM safe APIs  
- No eval/dynamic reflection of untrusted data  
- Context-aware output encoding:
  - HTML → escape
  - JSON → type-safe structured
  - Logs → exclude secrets/PII  
- Secrets never embedded in code  
- Generic error messaging externally

---

## 7. Mandatory Security Controls

### 7.1 Input Validation & Encoding
- Validate type, length, format  
- Reject unknown fields  
- Pydantic/DTO/schema-bound validation  
- Encoding per sink: HTML/JSON/logs

### 7.2 Authentication & Sessions
- Framework auth only  
- bcrypt / Argon2 / PBKDF2  
- Cookie flags: `HttpOnly`, `Secure`, `SameSite`  
- Do not log credentials/tokens

### 7.3 Authorization
- Server-side enforcement  
- RBAC/ABAC least privilege  
- Never rely on hidden URLs

### 7.4 Cryptography & Secrets
- Modern TLS & vetted crypto libs  
- Secrets from:
  - env vars
  - vaults
  - secure configuration  
- Never log keys/tokens

### 7.5 Error Handling & Logging
- No internal traces to clients  
- Generic outward errors  
- Logs sanitized of:
  - passwords
  - tokens
  - sensitive identifiers

### 7.6 Resource & DoS Protection
- Timeouts, quotas, pagination  
- Avoid unbounded operations  
- Close DB/file/network connections

### 7.7 Dependencies & APIs
- Maintained libs only  
- No eval-loaded plugins  
- Validate TLS certificates

---

## 8. ICL Prompt Structure

### 8.1 Zero-Shot

```text
Generate secure {language} code for {feature}, applying OWASP ASVS and secure defaults.
Enforce strict validation, parameterized queries, safe error handling,
and never include secrets in code or logs.
```

### 8.2 One-Shot

```text
[SECURE EXAMPLE]
- strict validation
- parameterized DB calls
- sanitized logging
- generic error responses

Now produce {feature} in {language/framework} matching the same controls.
```

### 8.3 Few-Shot

```text
[Example 1] validation + encoding
[Example 2] secure DB/file access
[Example 3] secure auth/token enforcement

Implement {feature} using these security baselines.
Prefer clarity & security over brevity.
```

---

## 9. Expected LLM Output Format

The assistant must provide:

1. **Secure Code Block**
2. **Security Summary**
   - validation strategy
   - injection prevention
   - secrets handling
   - logging safety
3. **Residual Risk Notes**
   - remaining assumptions
   - security TODOs

---

## 10. Developer Review Checklist

- SAST → **Snyk, Semgrep, Bandit, Sonar**
- No embedded secrets anywhere
- Strict input validation confirmed
- All operations parameterized
- Logging minimal & de-identified
- Errors generic, no stack traces
- Dependencies vetted & pinned
- Authorization enforced server-side
- Threat model matches deployment reality

---

## 11. “Never Allow” Rules

**Block merge if any of the following appear:**

- Hard-coded keys/tokens/passwords  
- TLS/HTTPS bypass or cert-skip flags  
- SQL/NoSQL concatenation of input  
- `eval`/reflection on untrusted data  
- Raw traces/log dumps to client  
- Unvalidated upload handling  
- Security check bypass “for debugging”

**🚫 Violation = Deployment Blocker**

---

## 12. References & Standards

- OWASP ASVS  
- OWASP Top 10  
- NIST SSDF (SP 800-218)  
- Microsoft SDL Secure Coding  
- CERT Secure Coding  
- ISO/IEC 27034  

---

**Usage Reminder:**  
This ICL template should be included in CI, DevSecOps pipelines, GitHub PR review rules, and LLM-assisted code generation workflows to produce repeatable enterprise-grade secure code across languages and runtime stacks.
