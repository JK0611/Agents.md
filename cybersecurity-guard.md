---
name: cybersecurity-guard
description: >-
  Enterprise-grade web application cybersecurity audit, OWASP Top 10 defense,
  MITRE ATT&CK compliance, HTTP security header verification, XSS sanitization,
  and privacy data protection guard for EPF Tracker.
---

# Cybersecurity Agent & Security Standard Manual

This skill acts as the autonomous security guardian for the **EPF Tracker** application. It enforces strict security posture across infrastructure, transport layer, frontend rendering, and privacy compliance.

---

## ⚠️ Non-Negotiable Coding Constraints & Guardrails
1. **HTML Sanitization**: Never inject unescaped external or user-provided strings into `innerHTML`. Always wrap dynamic values with `escapeHTML()`.
2. **CSP Compliance**: Never add arbitrary external `<script>` or `<link>` tags without updating `vercel.json`'s `Content-Security-Policy`.
3. **Link Security**: All external anchor tags (`<a>`) must include `target="_blank" rel="noopener noreferrer"`.
4. **Telemetry Privacy**: PostHog telemetry must maintain `maskAllInputs: true` and never capture plain-text personal credentials, IP addresses, or tokens.
5. **No Secret Ingestion**: Private keys, database credentials, and admin tokens must strictly reside in environment variables—never in client-side bundles.

---

## 1. Security Architecture & Threat Model

| Layer | Risk Surface | Defense Mechanism | Standard / Specification |
| :--- | :--- | :--- | :--- |
| **Edge / HTTP** | Clickjacking, MIME sniffing, Protocol Downgrade | Vercel Security Headers (`HSTS`, `X-Frame-Options: DENY`, `nosniff`) | [OWASP Secure Headers](https://owasp.org/www-project-secure-headers/) |
| **Execution Sandbox** | Script Injection, Malicious CDN Tampering | Strict Content Security Policy (`CSP` Whitelist) | [OWASP CSP Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Content_Security_Policy_Cheat_Sheet.html) |
| **DOM Rendering** | Stored & Reflected Cross-Site Scripting (XSS) | Universal Contextual Escaping (`escapeHTML()`) | [Cure53 DOMPurify Standards](https://github.com/cure53/DOMPurify) |
| **Telemetry & Analytics** | User PI/PII Exfiltration, Credential Leakage | PostHog Strict Privacy Masking (`maskAllInputs: true`) | [GDPR / PDPA Compliance](https://posthog.com/docs/privacy) |
| **API & Secrets** | Token Exposure, Unauthorized API Consumption | Serverless Env Isolation & Scoped Permissions | [NIST SP 800-53](https://csrc.nist.gov/) |

---

## 2. Automated Security Audit Checklist

Whenever new features, third-party libraries, or UI components are introduced, the agent MUST run through this verification checklist:

### A. Static Code Security Checks
1. **Zero Secret Leaks**: Verify no private tokens or API secrets are committed to client bundles.
2. **Safe Dynamic HTML**: Confirm all dynamic strings rendered via `innerHTML` or template literals pass through `escapeHTML()`.
3. **No Unsafe Sinks**: Reject the use of `eval()`, `new Function()`, `document.write()`, or unescaped `javascript:` pseudo-protocols in `href`.

### B. Network & Origin Verification
1. **CSP Integrity**: Ensure any new external resource (font, style, script, CDN) is explicitly declared in `vercel.json`'s `Content-Security-Policy`.
2. **Frame Sandboxing**: Ensure `X-Frame-Options: DENY` remains active to prevent phishing or clickjacking overlays.
3. **External Link Safety**: Ensure all external links (`klsescreener`, GitHub) utilize `target="_blank" rel="noopener noreferrer"`.

### C. Privacy & Compliance
1. **Input Masking**: Verify that all user inputs (search boxes, filters) remain masked or scrubbed in Session Replays.
2. **Zero Cookie Footprint**: Keep the client application 100% cookie-free and tracker-transparent.

---

## 3. Incident Response & Security Testing Commands

```powershell
# Verify Content-Security-Policy & Security Headers on Local/Staging
node frontend/test-runner.js

# Test XSS string resistance against live DOM elements
node -e "const { chromium } = require('playwright'); /* Automated penetration verification */"
```
