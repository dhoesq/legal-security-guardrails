---
description: View the session audit log (Free Tier -- view only, no export)
argument-hint: "[view]"
---

# /audit-log -- Session Audit Log (Free Tier)

View the security audit log for this session. Free tier supports viewing only. Export, search, and summary features require Pro.

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

Then proceed with the audit log display.

## Workflow

### Determine Action

Parse $ARGUMENTS:

- **No arguments or "view"**: Display the session audit log
- **"export"**: Display upgrade notice (Pro feature)
- **"search"**: Display upgrade notice (Pro feature)
- **"summary"**: Display upgrade notice (Pro feature)

### Action: View

Display all audit log entries in chronological order:

```
SESSION AUDIT LOG (Free Tier)
==============================
Session Start: [timestamp]

Entry #1 -- [timestamp]
  Action: [CLASSIFY/ANALYZE/etc.]
  Document: [reference]
  Classification: [level]
  [additional fields as applicable]

[... additional entries ...]

TOTAL ENTRIES: [count]

Note: Audit log export (Markdown, JSON, CSV), search, and summary
features are available in the Pro tier.
```

### Action: Export / Search / Summary (Pro Only)

If the user requests export, search, or summary, display:

```
FEATURE LOCKED: Audit log export, search, and summary are Pro features.

Pro audit log capabilities include:
- Export to Markdown, JSON, or CSV
- Search by document, client, matter, action type, or classification
- Session activity summary with compliance status
- 7-year retention format per ABA requirements

Upgrade: don@dhoesq.com | https://kaizenailab.com
```

## Notes

- Free tier displays the raw audit log only
- Pro tier adds export (MD/JSON/CSV), search, and summary with compliance status
- Each /audit-log invocation counts as 1 of 10 lifetime free uses
- Audit logs are maintained in session memory only (free tier does not persist logs)
