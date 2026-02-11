# Project History


## 2026-02-10 12:48 - Session Summary
Session in opex-technologies - no detailed summary available

---

## 2026-02-10 13:06 - Session Summary
Session in opex-technologies - no detailed summary available

---

## 2026-02-10 13:52 - Fixed Auth API Login Bug (Datetime Comparison Crash)

### Issue
Unable to log in to the Opex Form Builder UI at https://opex-technologies.github.io/login with `logintest@opextech.com` / `TestPassword123`. API returned generic "Login failed" 500 error.

### Root Cause
Two compounding issues:
1. **Account locked** — `logintest@opextech.com` had 5 failed login attempts, triggering the 15-minute lockout (`account_locked_until` set in Firestore)
2. **Datetime comparison bug** — `main.py:327` compared a timezone-aware datetime (from Firestore) with `datetime.utcnow()` (naive), causing `TypeError: can't compare offset-naive and offset-aware datetimes`. This meant locked accounts could **never** unlock — the check always crashed.

### Actions Taken
1. Switched to opex project, pulled latest from GitHub Pages repo
2. Tested login API via curl — confirmed 500 error
3. Downloaded auth-api Cloud Function source from GCS bucket
4. Read Cloud Function logs — identified the `TypeError` on line 327
5. Reset account lock in Firestore (cleared `failed_login_attempts` and `account_locked_until`)
6. Fixed datetime comparison in `/tmp/auth-api-source/main.py` — now checks `tzinfo` and uses `datetime.now(timezone.utc)` when comparing against tz-aware values
7. Redeployed auth-api Cloud Function (revision `auth-api-00018-pir`)
8. Verified login works via curl — confirmed success

### Files Modified
- `auth-api/main.py` (Cloud Function, deployed to GCP) — Fixed timezone-aware vs naive datetime comparison in account lock check

### Key Findings
- Users are stored in Firestore `users` collection, keyed by email
- Auth API: `https://us-central1-opex-data-lake-k23k4y98m.cloudfunctions.net/auth-api`
- Form Builder API: `https://us-central1-opex-data-lake-k23k4y98m.cloudfunctions.net/form-builder-api`
- Response Scorer API: `https://us-central1-opex-data-lake-k23k4y98m.cloudfunctions.net/response-scorer-api`
- `test@opextech.com` also has 5 failed attempts and is locked — same bug would affect that account
- The `create_admin.py` script references BigQuery (legacy) but the live system uses Firestore
- The GCP project credentials file path in `switch-gcp-project.sh` references `/Users/landoncolvig/` which doesn't exist on this machine

### Outstanding
- **Cloud Function source not in git** — The auth-api fix was deployed from `/tmp/auth-api-source/` (downloaded from GCS). There is no git repo backing the Cloud Function source code. Should create one to avoid losing changes on future deploys.
- **`test@opextech.com` also locked** — Has 5 failed attempts, needs lock reset if that account is needed
- **Switch script broken** — `~/.claude/scripts/switch-gcp-project.sh` has hardcoded credential path for old machine; needs updating for this machine

---

## 2026-02-10 13:07 - Session Summary
Session in opex-technologies - no detailed summary available

---

## 2026-02-10 19:38 - Session Summary
Session in opex-technologies - no detailed summary available

---
