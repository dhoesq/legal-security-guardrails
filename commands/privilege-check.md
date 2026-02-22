---
description: View session security status (Free Tier -- read-only)
---

# /privilege-check -- Session Security Status (Free Tier)

Display the current session security status. Free tier provides a read-only status view without remediation recommendations or compliance scoring.

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

Then proceed with the status display.

## Workflow

### Step 1: Display Basic Session Status

Present a status report:

```
SESSION SECURITY STATUS (Free Tier)
====================================

Session Start: [timestamp]
Classification Levels Used: [list -- PUBLIC/INTERNAL only in free tier]

DOCUMENTS PROCESSED THIS SESSION:
1. [Document 1] -- [CLASSIFICATION] -- [timestamp]
2. [Document 2] -- [CLASSIFICATION] -- [timestamp]
(or: No documents processed)

AUDIT LOG ENTRIES: [count]

FREE TIER LIMITATIONS:
- Classification: PUBLIC and INTERNAL only
- No privilege protection or ethical walls
- No compliance scoring (GREEN/YELLOW/RED)
- No remediation recommendations
- No consent registry
- No conflict registry

Upgrade to Pro for full privilege status verification, compliance
scoring, consent tracking, conflict detection, and remediation
recommendations.

Contact: don@dhoesq.com | https://kaizenailab.com
```

### Step 2: Log the Check

Record an audit log entry:
- Action: PRIVILEGE_CHECK
- Result: FREE_TIER_VIEW_ONLY
- Timestamp

## Notes

- Free tier provides view-only status with no health scoring or recommendations
- Pro tier includes GREEN/YELLOW/RED health checks with specific remediation steps
- Each /privilege-check invocation counts as 1 of 10 lifetime free uses
