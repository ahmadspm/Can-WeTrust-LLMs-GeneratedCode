# Universal DevSecOps ICL Prompt Template for AI Coding Agents

## Purpose
This template enables secure-by-design AI-assisted code generation in software engineering and DevSecOps workflows. It is compatible with Prompt-Driven Code Generators (PDCGs), Code Completion Platforms (CCPs), IDE assistants, and CI/CD pipelines.

---

## 1. Software Engineering Context
Project: [INSERT]

System / Component: [INSERT]

SDLC Stage: [Requirements | Design | Implementation | Testing | Refactoring | Maintenance]

Environment: [IDE | Copilot | CI/CD | Agent | CLI | Other]

Language / Framework: [INSERT]

Task Type: [API | MVC | CLI | Data Processing | Auth | File Handling | Other]

---

## 2. Functional Task
Task Description:
[INSERT]

Expected Output:
[INSERT]

---

## 3. Security Requirements
The generated code MUST:

- validate all untrusted inputs
- avoid hard-coded secrets
- enforce authentication/authorization where required
- prevent common vulnerabilities
- avoid sensitive data leakage
- follow least privilege and secure defaults
- produce production-ready maintainable code

Additional requirements:
[INSERT]

Standards:
- OWASP ASVS: [INSERT]
- NIST SSDF: [INSERT]
- CWE(s): [INSERT]

---

## 4. Threat Context
Threats:
- [INSERT]
- [INSERT]

Avoid:
- CWE-20 (Input Validation)
- CWE-22 (Path Traversal)
- CWE-79 (XSS)
- CWE-89 (SQL Injection)
- CWE-200 (Info Exposure)
- CWE-798 (Hardcoded Secrets)

---

## 5. Secure Generation Instructions
1. Identify risks
2. Apply secure design
3. Generate secure code
4. Explain security decisions
5. Suggest validation tests

---

## 6. Output Format

### A. Security Risks
[Brief]

### B. Secure Code
[Implementation]

### C. Security Explanation
[Brief]

### D. Checklist
- [ ] Input validation
- [ ] Secrets protected
- [ ] Auth handled
- [ ] Safe error handling
- [ ] No unsafe functions
- [ ] Logging safe

### E. Security Tests
- unit tests
- negative tests
- static analysis
- dependency scan

---

## Learning Modes

### Zero-shot
Use as-is

### One-shot
Add one secure example

### Few-shot
Add multiple secure examples

---

## DevSecOps Integration

Use in:
- IDE prompts
- CI/CD policies
- PR automation
- Secure coding guidelines