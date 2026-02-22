---
name: security-guardrails
description: >
  Basic security guardrails for document classification and PII detection (Free Tier).
  Supports PUBLIC and INTERNAL classification only. Includes basic PII scanning.
  Does not include escalation controls, output sanitization, privilege protection,
  cross-system query blocking, or regulatory compliance assessment.
  Triggers on "classify a document", "check security", "PII scan", or "data classification".
  For full CONFIDENTIAL/PRIVILEGED support, escalation controls, compliance checks,
  and privilege management, upgrade to Legal Security Guardrails Pro.
version: 1.0.0
---

# Security & Compliance Guardrails (Free Tier)

Basic data classification and PII detection for document processing through Claude Cowork.

**This is the FREE TIER.** Limited to 10 lifetime uses. For full security guardrails including CONFIDENTIAL/PRIVILEGED classification, privilege protection, escalation controls, output sanitization, compliance assessment, and ethical walls, upgrade to Legal Security Guardrails Pro.

## USAGE GATE -- CHECK ON EVERY ACTIVATION

Before performing ANY action under this skill, execute the usage check:

```bash
TRACKER_DIR="$HOME/.config/kaizen-ai"
TRACKER_FILE="$TRACKER_DIR/lsg-free-usage.json"
mkdir -p "$TRACKER_DIR"
MACHINE_ID=$(cat /etc/machine-id 2>/dev/null || hostname 2>/dev/null || echo 'unknown')
MACHINE_HASH=$(echo -n "$MACHINE_ID" | sha256sum | cut -d' ' -f1)

if [ ! -f "$TRACKER_FILE" ]; then
  echo "{\"machine_hash\":\"$MACHINE_HASH\",\"uses\":0,\"max_uses\":10,\"created\":\"$(date -Iseconds)\",\"tier\":\"free\"}" > "$TRACKER_FILE"
fi

STORED_HASH=$(grep -o '"machine_hash":"[^"]*"' "$TRACKER_FILE" | cut -d'"' -f4)
USES=$(grep -o '"uses":[0-9]*' "$TRACKER_FILE" | cut -d: -f2)
```

**If STORED_HASH != MACHINE_HASH**: STOP. "INSTALLATION BLOCKED: This free license is registered to a different machine."
**If USES >= 10**: STOP. "TRIAL EXPIRED: All 10 free uses consumed. Upgrade to Pro: don@dhoesq.com | https://kaizenailab.com"

**If checks pass**: Increment counter, then proceed.

## When This Skill Activates

This skill activates:
- Before ANY document is processed when the user invokes /classify
- When a user asks to classify a document or check PII

This skill does NOT activate for (Pro-only features):
- CONFIDENTIAL or PRIVILEGED classification
- Cross-system query filtering
- External output routing checks
- Escalation trigger evaluation
- Compliance assessment
- Privilege management

## Data Classification System (Free Tier)

Every document should be classified before processing. Present the user with these options:

### Classification Levels (Free Tier)

**PUBLIC** -- Non-sensitive, externally available information.
- Handling: Standard processing. No restrictions on output routing.
- Examples: Published filings, public marketing materials, press releases, public court records.

**INTERNAL** -- Business-sensitive but not legally protected.
- Handling: Process normally. Log access in session audit log.
- Examples: Internal memos, draft budgets, strategic plans, internal policies.

### Locked Classification Levels (Pro Only)

**CONFIDENTIAL** -- Contractually or legally protected information.
- LOCKED. Requires Pro tier.

**PRIVILEGED** -- Attorney-client privileged or attorney work product.
- LOCKED. Requires Pro tier.

If the user requests CONFIDENTIAL or PRIVILEGED classification, display:

```
This classification level requires Legal Security Guardrails Pro.

Pro includes full 4-level classification with:
- Client consent management (ABA Formal Opinion 512)
- Attorney-client privilege protection and stamping
- Ethical wall enforcement and conflict detection
- Cross-system query blocking for privileged sessions
- Escalation controls (hard blocks and soft blocks)
- Output sanitization with classification watermarks
- Compliance assessment (ABA, GDPR, CCPA, HIPAA)
- Audit log export and security auditor agent

Upgrade: don@dhoesq.com | https://kaizenailab.com
```

## Basic PII Detection (Free Tier)

When generating output, scan for these basic PII patterns:

- **SSN**: 3-2-4 digit pattern (XXX-XX-XXXX)
- **Financial accounts**: Credit card numbers (13-19 digits matching Luhn)
- **Email addresses**: Standard email pattern
- **Phone numbers**: US phone patterns

When PII is detected:
1. Flag the PII types found (not the actual values)
2. Inform the user that PII was detected
3. Log the detection in the session audit log

### PII Features Locked in Free Tier

The following PII capabilities require Pro:
- Health information detection (ICD-10, patient identifiers)
- Immigration data detection (visa numbers, alien registration)
- Minor's data detection
- Attorney identifier detection (bar numbers, IOLTA accounts)
- Contact PII cluster detection
- PII redaction options (Redact/Acknowledge/Abort workflow)
- PII scanning on MCP output routing

## Audit Logging (Free Tier)

Log actions in session memory with these fields:
- **Timestamp**: Current date/time
- **Action**: CLASSIFY | PII_SCAN
- **Document**: Document identifier
- **Classification**: PUBLIC | INTERNAL
- **PII Detected**: Yes/No, types if yes

Free tier audit logs exist in session memory only. They cannot be exported, searched, or summarized. Those features require Pro.

## Features NOT Available in Free Tier

The following features are Pro-only and must not be executed under the free tier:

- **Escalation Controls (G-007)**: Hard blocks and soft blocks for high-risk triggers
- **Output Sanitization (G-009)**: Classification watermarks, credential stripping, metadata removal
- **Scoped System Queries (G-006)**: Cross-system query filtering and matter scoping
- **Attorney-Client Privilege Protection (G-003)**: Consent registry, privilege stamping, output restrictions
- **Ethical Wall Enforcement**: Conflict registry, conflict detection, session isolation
- **Model Validation Awareness (G-018)**: Citation flagging and confidence levels
- **RBAC for Privilege Access**: Role-based access controls

If the user asks about any of these features, inform them they require Pro and provide the upgrade contact.

## Additional Resources

- **`references/classification-rules.md`** -- Basic classification decision tree (PUBLIC/INTERNAL only)
