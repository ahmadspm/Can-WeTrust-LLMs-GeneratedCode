# ICL Security Pattern Template – Secure Coding with LLMs

**Version:** 1.0  
**Scope:** Language-agnostic, framework-agnostic  
**Use:** LLM-assisted secure coding (ICL patterns) in DevSecOps pipelines

---

## Table of Contents

- [1. Purpose](#1-purpose)
- [2. LLM Role Setup](#2-llm-role-setup)
- [3. Development Context](#3-development-context)
- [4. Trust & Threat Model](#4-trust--threat-model)
- [5. Security Objectives](#5-security-objectives)
- [6. Functional & Security Specification](#6-functional--security-specification)
- [7. Mandatory Security Controls](#7-mandatory-security-controls)
- [8. ICL Prompt Structure](#8-icl-prompt-structure)
- [9. Expected LLM Output Format](#9-expected-llm-output-format)
- [10. Developer Review Checklist](#10-developer-review-checklist)
- [11. “Never Allow” Rules](#11-never-allow-rules)
- [12. References & Standards](#12-references--standards)

---

## 1. Purpose

This template standardises how developers and AI coding systems (LLMs, GitHub Copilot, ChatGPT, CodeWhisperer, etc.) apply **secure-by-design** principles when generating code using **In-Context Learning (ICL) Security Patterns**.

It is designed to:

- Enforce **secure defaults** and explicit risk awareness  
- Minimise vulnerability surface across languages and paradigms  
- Provide **consistent secure coding behaviour** for LLM-assisted workflows  
- Support **repeatable, auditable, and defensible** security posture in DevSecOps pipelines  

---

## 2. LLM Role Setup

**Instruction to the model (paste at the top of your prompt):**

```text
You are a <secure coding assistant>. Your priorities are, in this exact order:
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

## 3. Development Context

Fill this in before you ask the LLM to generate code.

Feature / Module

Example: Secure login endpoint, audit logger, firmware update verifier, payment API

Your description:
{Describe what needs to be built}

### Language & Framework
{Python + FastAPI, C# ASP.NET Core, Java Spring, Node.js Express, Go Gin,
 C++ CLI, Rust Actix, etc.}

### Runtime / Platform
{e.g. .NET 8, JVM 17, Python 3.11, AWS Lambda, Azure App Service, Kubernetes,
 on-prem microservice, ICS gateway, etc.}

### Data Sensitivity (choose one)

 Public

 Internal

 Confidential

 Secret / Highly regulated

Threat Profile (choose one)

 Low – dev tools, prototypes, non-sensitive data

 Medium – standard business data

 High – auth, payments, safety-critical, PII/health, OT/ICS

Domain

{Web / API / Microservice / Mobile / Desktop / ICS / OT / Cloud-native / CLI}


## 4. Trust & Threat Model
Trust Boundaries

Untrusted:
Internet clients, uploaded files, user-controlled headers, query params, body, external public APIs.

Semi-trusted:
Internal services, partner APIs, message queues, internal dashboards.

Trusted:
Databases, key vaults, HSMs, identity providers, secrets management systems.

Threat Concerns (tick applicable)

 Injection (SQL / NoSQL / Command / Template / LDAP)

 XSS / HTML injection

 CSRF / session fixation

 IDOR / broken access control

 SSRF / LFI / RFI

 Unsafe deserialization / reflection

 Path traversal / arbitrary file access

 Insecure crypto & hard-coded keys

 Sensitive data exposure (logs, responses, crash dumps)

 Supply chain misuse (unvetted libraries, plugins)

 DoS / resource exhaustion

## 5. Security Objectives

Mark the properties the implementation must uphold:

 Confidentiality – protect secrets and sensitive data

 Integrity – prevent tampering, injection, logic abuse

 Availability – timeouts, rate limits, graceful degradation

 Authentication – identity proof, tokens, sessions

 Authorization – RBAC / ABAC, least privilege

 Traceability & Auditing – logs, correlation IDs, non-repudiation

 Safety / ICS Process Protection – for OT / CPS / safety-critical logic

## 6. Functional & Security Specification
6.1 Functional Steps (developer defines)
1. {Receive request/input – HTTP/CLI/message/etc.}
2. {Validate input schema and types, reject invalid payloads}
3. {Perform business logic / DB queries / external calls}
4. {Return structured response with safe error handling}
6.2 Security Requirements (LLM must implement)

Strict, preferably whitelisting-based input validation

Parameterised DB access or safe ORM usage (no string concatenation with untrusted input)

No dynamic eval or unsafe reflection on untrusted data

Context-aware output encoding:

HTML → HTML-encode

JSON → correct types, no raw HTML / scripts

Logs → avoid secrets / PII

Secrets never hard-coded or logged

Generic, non-verbose error messages to clients


## 7. Mandatory Security Controls
7.1 Input Validation & Encoding

Validate type, length, format (email, UUID, etc.)

Reject unexpected fields (fail closed, not open)

Use schema validators / DTOs / pydantic / annotations / model binding

Encode outputs based on sink:

HTML output → HTML-escape

JSON → safe serialisation

Logs → strip sensitive fields

7.2 Authentication & Session Management

Use framework-provided auth/session mechanisms; no custom crypto

Hash passwords with vetted algorithms (bcrypt, Argon2, PBKDF2) via standard libs

Use secure cookie flags (HttpOnly, Secure, SameSite) where appropriate

Never log passwords or full tokens

7.3 Authorization

Enforce server-side access control on every sensitive action

Use RBAC/ABAC and principle of least privilege

Do not rely on client-side checks or hidden URLs

7.4 Cryptography & Secrets

Use vetted crypto libraries and modern TLS versions

No home-grown encryption/hashing schemes

Load secrets from:

environment variables

secret stores / vaults

secure config providers

Never hard-code API keys, tokens, or DB passwords in code

7.5 Error Handling & Logging

Do not include stack traces, file paths, queries, or secrets in client-facing errors

Return generic error messages; log detailed errors internally

Avoid logging:

passwords

complete tokens

high-sensitivity PII

7.6 Resource Management & DoS

Apply:

timeouts

input/payload size limits

pagination or streaming for large results

Close DB connections, file handles, network streams

Avoid unbounded loops or allocations based on untrusted data

7.7 Dependencies & External APIs

Prefer maintained, well-known libraries

Avoid unsafe dynamic plugin loading or eval-based extension mechanisms

Enforce TLS certificate and hostname validation for outbound calls

## 8. ICL Prompt Structure

Use these patterns when prompting LLMs (locally, in CI, or in DevOps tooling).

8.1 Zero-Shot (baseline secure prompt)
Generate secure {language} code for {feature}, applying OWASP ASVS and
secure-by-default practices: strict input validation, parameterized DB/API access,
safe error handling, and no hard-coded secrets.

8.2 One-Shot (add one secure example before the request)
[SECURE EXAMPLE – ONE-SHOT]
Uses:
- strict input schema validation
- parameterized query
- generic error messages
- no sensitive data in logs

Now, using the same secure coding style, implement:
{your new feature description} in {language/framework}.

8.3 Few-Shot (2–3 focused secure examples)
[Example 1] Secure validation + encoding
[Example 2] Secure DB / file access
[Example 3] Secure auth / API key check

Using these examples as security patterns, implement {feature} in {language/framework}.
If you must trade off, favor security and clarity over brevity.

## 9. Expected LLM Output Format

The LLM should respond with three clearly separated parts:

Secure Implementation Code

Self-contained snippet (or minimal set of files)

Only essential security-related comments (// SECURITY: style)

Security Checklist Summary
Short bullets describing:

How inputs are validated and sanitised

How injection and other key threats are mitigated

How secrets and credentials are handled

How errors and logging avoid sensitive data leaks

Known Limitations / Security TODOs

Explicit assumptions and trade-offs

Edge cases not fully handled

Items requiring manual review or hardening

## 10. Developer Review Checklist

Before merge or deployment, a human reviewer should confirm:

 SAST tools (Snyk, Semgrep, Bandit, Sonar, etc.) run clean or with understood, documented exceptions

 No secrets, API keys, or credentials in code, config, or logs

 All external inputs are validated (type, length, allowed values, ranges)

 All DB, file, and external calls use parameterisation or safe APIs

 Logging is appropriate and does not expose PII or sensitive security data

 Error responses to clients are generic and non-verbose

 Dependencies are pinned and from trusted sources/registries

 Authorization logic is enforced server-side and covered by tests

 Threat model assumptions match the actual deployment (trust boundaries, exposure, integrations)

Review tip: You can ask the LLM to identify potential CWEs in its own output, then manually validate and fix them.

## 11. “Never Allow” Rules

The generated code and review process must never accept any of the following:

Hard-coded passwords, tokens, keys, or secrets of any kind

Disabling TLS/HTTPS or skipping certificate validation in production code

Building SQL/NoSQL/LDAP queries by string concatenation of untrusted input

Using eval / reflection / dynamic execution on untrusted input

Returning raw stack traces, file system paths, or SQL queries to clients

Accepting file uploads without validation, size limits, or storage controls

Bypassing access control checks “for convenience” or “temporary debugging”

Policy: Breaking any “Never Allow” rule must block the merge or deployment until fixed, or be explicitly risk-accepted and documented under organisational governance.

## 12. References & Standards

Recommended baselines for implementing and extending this template:

OWASP Application Security Verification Standard (ASVS)

OWASP Top 10 (latest edition)

NIST Secure Software Development Framework (SSDF), SP 800-218

CERT Secure Coding Standards

Microsoft Security Development Lifecycle (SDL) – Secure Coding Guidelines

ISO/IEC 27034 – Application Security