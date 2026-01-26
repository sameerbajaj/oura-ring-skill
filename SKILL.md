---
name: oura-ring
description: Humanized Oura V2 integration for tactical readiness coaching and trend analysis.
---

# Oura Ring (The Insight Engine)

This skill isn't for tracking numbers; it's for managing your biological budget. It turns the Oura V2 API into a set of tactical instructions for your daily life. 

## Capability Summary
Use this when you want to know if you should push through a deep-work sprint or back off for recovery. It interprets sleep architecture, heart rate deltas, and HRV trends to tell you the *why* behind your scores.

## Quick Commands
*   *The Brief*: `bash skills/oura-ring/scripts/morning_brief.sh` (The "Human-First" summary).
*   *Raw Data*: `python3 skills/oura-ring/cli.py [readiness|sleep|trends|resilience|stress]`

## The "One Big Prompt" Integration
If you are spawning a subagent to help plan your week, feed it the morning brief first. It will automatically suggest moving high-intensity tasks to high-readiness days.

## Setup
1.  *Token*: Put your Oura Personal Access Token in `skills/oura-ring/.env` as `OURA_PERSONAL_ACCESS_TOKEN=xxx`.
2.  *Privacy*: This skill processes everything locally. Your token never leaves your machine.

### 1) Install dependencies (recommended: venv)

macOS/Homebrew Python often blocks system-wide `pip install` (PEP 668), so use a virtualenv:

```bash
python3 -m venv skills/oura-ring/.venv
source skills/oura-ring/.venv/bin/activate
python -m pip install -r skills/oura-ring/requirements.txt
```

### 2) Create your `.env`

Create `skills/oura-ring/.env`:

```bash
cp skills/oura-ring/.env.example skills/oura-ring/.env
# then edit skills/oura-ring/.env
```

The CLI reads:
- `OURA_TOKEN` (required)
- `OURA_BASE_URL` (optional; defaults to `https://api.ouraring.com/v2/usercollection`)

## Getting an Oura token (OAuth2)

Oura V2 uses OAuth2 bearer tokens.

1. Create an Oura API application:
   - https://cloud.ouraring.com/oauth/applications
2. Set a Redirect URI (for local testing, something like `http://localhost:8080/callback`).
3. Open the authorization URL (replace `CLIENT_ID`, `REDIRECT_URI`, and `scope`):

```text
https://cloud.ouraring.com/oauth/authorize?response_type=code&client_id=CLIENT_ID&redirect_uri=REDIRECT_URI&scope=readiness%20sleep
```

4. After approving, you’ll be redirected to your Redirect URI with a `code=...` query parameter.
5. Exchange the code for an access token:

```bash
curl -X POST https://api.ouraring.com/oauth/token \
  -H 'Content-Type: application/x-www-form-urlencoded' \
  -d grant_type=authorization_code \
  -d client_id=CLIENT_ID \
  -d client_secret=CLIENT_SECRET \
  -d redirect_uri=REDIRECT_URI \
  -d code=AUTH_CODE
```

6. Put the returned `access_token` into `skills/oura-ring/.env` as `OURA_TOKEN=...`.

Notes:
- Access tokens can expire; you may need to refresh using the `refresh_token`.
- Do **not** commit your `.env` file.

## Usage

### Readiness

```bash
python3 skills/oura-ring/cli.py --env-file skills/oura-ring/.env --format json --pretty readiness
```

### Sleep

```bash
python3 skills/oura-ring/cli.py --env-file skills/oura-ring/.env --format json --pretty sleep
```

### Trends (last 7 days; paginated)

```bash
python3 skills/oura-ring/cli.py --env-file skills/oura-ring/.env --format json --pretty trends
```

## Wrapper: Morning Readiness Brief

```bash
./skills/oura-ring/scripts/morning_brief.sh
```

Override the env file location:

```bash
OURA_ENV_FILE=/path/to/.env ./skills/oura-ring/scripts/morning_brief.sh
```

Run in mock mode (no token):

```bash
OURA_MOCK=1 ./skills/oura-ring/scripts/morning_brief.sh
```

## Verification (no token required)

```bash
python3 skills/oura-ring/cli.py --mock readiness --format json
python3 skills/oura-ring/cli.py --mock sleep --format json
python3 skills/oura-ring/cli.py --mock trends --format json
```
