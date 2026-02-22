# Legal Security Guardrails (Free Tier)

Basic security and compliance guardrails for law firms, financial services, and healthcare organizations using Claude Cowork for document review.

**Free Tier: 10 lifetime uses, then upgrade required.**

## What This Plugin Does

This free tier provides a preview of the Legal Security Guardrails plugin:

- **Basic data classification** (PUBLIC / INTERNAL) before document processing
- **Basic PII detection** scanning for SSN, credit cards, emails, and phone numbers
- **Session audit log** (view-only) of classification and PII events
- **Session security status** (read-only view)

## What's Locked (Pro Only)

The following features require Legal Security Guardrails Pro:

| Feature | Free | Pro |
|---------|------|-----|
| PUBLIC / INTERNAL classification | Yes | Yes |
| CONFIDENTIAL / PRIVILEGED classification | No | Yes |
| Basic PII detection (SSN, CC, email, phone) | Yes | Yes |
| Advanced PII (health, immigration, minor's data) | No | Yes |
| PII redaction workflow | No | Yes |
| Audit log view | Yes | Yes |
| Audit log export (MD, JSON, CSV) | No | Yes |
| Audit log search and summary | No | Yes |
| Attorney-client privilege protection | No | Yes |
| Privilege stamping on outputs | No | Yes |
| Client consent management (ABA Op. 512) | No | Yes |
| Ethical wall enforcement | No | Yes |
| Conflict-of-interest detection | No | Yes |
| Escalation controls (hard/soft blocks) | No | Yes |
| Output sanitization | No | Yes |
| Cross-system query blocking | No | Yes |
| Secure contract review (/secure-review) | No | Yes |
| Compliance assessment (/compliance-check) | No | Yes |
| Consent form generation (/consent-form) | No | Yes |
| Security auditor agent | No | Yes |
| MCP guardrail hooks | No | Yes |
| Write/Edit guardrail hooks | No | Yes |
| RBAC for privilege access | No | Yes |
| Usage limit | 10 uses | Unlimited |

## Commands

| Command | Description | Limit |
|---------|-------------|-------|
| `/classify` | Classify documents (PUBLIC/INTERNAL only) | 10 uses total |
| `/privilege-check` | View session security status | 10 uses total |
| `/audit-log` | View session audit log | 10 uses total |

## Usage Tracking

This plugin tracks usage with a machine-locked counter:

- **10 lifetime uses** across all commands and skill activations
- **Machine-locked**: The free license is tied to the first machine that installs it
- **Non-transferable**: Cannot be reinstalled on a different machine
- After 10 uses, all commands and the skill are disabled

## Upgrade to Pro

For unlimited usage and full security guardrails:

**Legal Security Guardrails Pro** includes everything in Free, plus CONFIDENTIAL/PRIVILEGED classification, privilege management, ethical walls, compliance assessment, consent forms, secure contract review, security auditor agent, and all guardrail hooks.

Contact: info@kaizenailab.com | https://kaizenailab.com

Developed by Kaizen AI Lab (CVDH LLC).
