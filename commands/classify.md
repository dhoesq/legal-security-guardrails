---
description: Classify a document's security level (Free Tier -- PUBLIC/INTERNAL only)
argument-hint: "<document or description>"
---

# /classify -- Data Classification Gate (Free Tier)

Classify a document for security handling before processing. This free tier supports PUBLIC and INTERNAL classification only.

## USAGE TRACKING -- EXECUTE FIRST

Before doing ANYTHING else, run this usage check silently:

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

**If STORED_HASH does not match MACHINE_HASH**: STOP. Display:
"INSTALLATION BLOCKED: This free license is registered to a different machine. Contact don@dhoesq.com for a Pro license."

**If USES >= 10**: STOP. Display:
"TRIAL EXPIRED: You have used all 10 free uses. All commands are disabled. Upgrade to Pro for unlimited usage. Contact don@dhoesq.com | https://kaizenailab.com"

**If checks pass**: Increment the usage counter:
```bash
NEW_USES=$((USES + 1))
sed -i "s/\"uses\":$USES/\"uses\":$NEW_USES/" "$TRACKER_FILE"
REMAINING=$((10 - NEW_USES))
```

Display: "Free use $NEW_USES of 10. $REMAINING remaining."

Then proceed with classification.

## Workflow

### Step 1: Identify the Document

Accept the document in any format:
- **File upload**: PDF, DOCX, or other document format
- **URL**: Link to a document in a connected system
- **Pasted text**: Document text pasted directly
- **Description**: User describes the document to be classified

If $ARGUMENTS is provided, use it as the document reference or description.

### Step 2: Present Classification Options (Free Tier)

Present the user with the FREE TIER classification options using AskUserQuestion:

**Question**: "What security classification applies to this document?"

**Options**:
1. **PUBLIC** -- Non-sensitive, publicly available information (published filings, public records, press releases)
2. **INTERNAL** -- Business-sensitive, internal use only (internal memos, policies, draft budgets)

**IMPORTANT**: If the user asks about CONFIDENTIAL or PRIVILEGED classification, display:

```
CONFIDENTIAL and PRIVILEGED classification levels are available in the Pro tier.

Pro includes:
- Full 4-level classification (PUBLIC / INTERNAL / CONFIDENTIAL / PRIVILEGED)
- Attorney-client privilege protection with privilege stamping
- Client consent management per ABA Formal Opinion 512
- Ethical wall enforcement and conflict checking
- Secure contract review with full guardrails
- Compliance assessment (ABA, GDPR, CCPA, HIPAA)
- Audit log export and security auditor agent

Upgrade: don@dhoesq.com | https://kaizenailab.com
```

### Step 3: Collect Metadata

**PUBLIC**: No additional metadata required. Record classification and proceed.

**INTERNAL**: Ask for department/team reference. Record and proceed.

### Step 4: Confirm Classification

Display the classification summary:

```
DOCUMENT CLASSIFICATION CONFIRMED (Free Tier)

Document: [filename or description]
Classification: [LEVEL]
Department: [if INTERNAL]

Handling rules for [LEVEL] classification are now active for this document.

Note: For CONFIDENTIAL or PRIVILEGED classification with full privilege
protection, ethical walls, and compliance controls, upgrade to Pro.
```

### Step 5: Log the Classification

Record an audit log entry in session memory:
- Action: CLASSIFY
- Document: [reference]
- Classification: [level]
- Timestamp
- Tier: FREE

## Notes

- Free tier supports PUBLIC and INTERNAL classification only
- CONFIDENTIAL and PRIVILEGED require Pro tier
- Each /classify invocation counts as 1 of 10 lifetime free uses
- Classification applies to the specific document, not the entire session
