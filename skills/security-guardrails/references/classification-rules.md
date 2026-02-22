# Classification Rules (Free Tier)

## Decision Tree

```
Is the document publicly available?
├── YES → PUBLIC
│   Examples: SEC filings, court records, press releases,
│   published marketing materials, public website content
│
└── NO → Is it legally or contractually protected?
    ├── NO → INTERNAL
    │   Examples: Internal memos, draft budgets, strategic plans,
    │   employee handbooks, meeting notes, internal reports
    │
    └── YES → REQUIRES PRO TIER
        ├── Client contracts → CONFIDENTIAL (Pro)
        ├── NDAs → CONFIDENTIAL (Pro)
        ├── Financial records → CONFIDENTIAL (Pro)
        ├── Employee PII → CONFIDENTIAL (Pro)
        ├── Attorney-client communications → PRIVILEGED (Pro)
        ├── Legal analysis → PRIVILEGED (Pro)
        ├── Litigation strategy → PRIVILEGED (Pro)
        └── Settlement discussions → PRIVILEGED (Pro)
```

## Classification Handling (Free Tier)

| Level | Processing | Output Routing | Audit | Consent |
|-------|-----------|---------------|-------|---------|
| PUBLIC | Standard | No restrictions | Basic log | Not required |
| INTERNAL | Standard | Log access | Basic log | Not required |
| CONFIDENTIAL | Pro only | -- | -- | -- |
| PRIVILEGED | Pro only | -- | -- | -- |

## Upgrade Path

For CONFIDENTIAL and PRIVILEGED classification with full privilege protection, ethical walls, compliance controls, and audit export:

**Legal Security Guardrails Pro**
Contact: don@dhoesq.com | https://kaizenailab.com
