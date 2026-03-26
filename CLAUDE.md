# Opex Technologies

## Project Overview
Form builder application with authentication, scoring engine, and PDF export. SPA frontend deployed to GCS (production) and GitHub Pages (secondary) with Cloud Function backends on GCP.

## Tech Stack
- **Frontend**: React/JSX (Vite) → GCS bucket + GitHub Pages
- **Backend**: Python Cloud Functions (GCP)
- **Database**: BigQuery (`form_builder` dataset), Firestore (auth)
- **GCP Project**: `opex-data-lake-k23k4y98m`

## Key URLs
- **Production UI**: https://opexiq.opextechnologies.com/form-builder/ (GCS + Load Balancer)
- **Secondary UI**: https://opex-technologies.github.io
- **APIs**: `us-central1-opex-data-lake-k23k4y98m.cloudfunctions.net`

## Cloud Functions
- `auth-api` (Gen2) — User authentication (Firestore-backed, account lockout)
- `form-builder-api` (Gen2) — Template/question CRUD, form deployment
- `response-scorer-api` (**Gen1** — do NOT use `--gen2` flag) — Assessment scoring engine

## Deployment

### Frontend (BOTH targets required)
The production app runs on `opexiq.opextechnologies.com`, served from GCS. GitHub Pages is a secondary host. **Always deploy to both:**

```bash
# 1. Build
cd "Q4 form scoring project/frontend/form-builder" && npm run build

# 2. Deploy to GCS (production)
gsutil -m cp -r dist/assets/* gs://opex-deployed-forms/form-builder/assets/
gsutil cp dist/index.html gs://opex-deployed-forms/form-builder/index.html

# 3. Deploy to GitHub Pages (secondary)
cp -r dist/assets/* ~/Documents/opex-technologies/assets/
cp dist/index.html ~/Documents/opex-technologies/index.html
cd ~/Documents/opex-technologies && git add -A && git commit -m "msg" && git push
```

### Cloud Functions
```bash
# form-builder-api (Gen2)
gcloud functions deploy form-builder-api --project opex-data-lake-k23k4y98m \
  --region us-central1 --runtime python311 --trigger-http --allow-unauthenticated \
  --entry-point form_builder_handler --gen2 \
  --set-env-vars "JWT_SECRET_KEY=94_ZkQrzjLcZxefMEcFrxFBdlCK5YTpG777czsv-m-A,FORMS_BUCKET=opex-deployed-forms"

# response-scorer-api (Gen1 — NO --gen2 flag!)
gcloud functions deploy response-scorer-api --project opex-data-lake-k23k4y98m \
  --region us-central1 --runtime python311 --trigger-http --allow-unauthenticated \
  --entry-point response_scorer_handler \
  --set-env-vars "JWT_SECRET_KEY=94_ZkQrzjLcZxefMEcFrxFBdlCK5YTpG777czsv-m-A"
```

## Key Contacts
- **Adam Trzonkowski** — VP Solutions Engineering (primary)
- **Ben Thornton** — Domain/DNS
- **Ken Nowell** — Technical

## Repositories
- **This repo** (`opex-technologies/opex-technologies.github.io`) — Frontend SPA (GitHub Pages)
- **Backend repo** (`~/Documents/opex-technologies-src`) — Cloud Function source code (`landoncolvig/opex-technologies`)
- **Frontend source**: `~/Documents/opex-technologies-src/Q4 form scoring project/frontend/form-builder/`

## Notes
- SPA routing uses 404.html redirect pattern
- Account lockout uses datetime comparison (known previous bug area)
- GCS bucket `opex-deployed-forms` hosts both deployed forms (`/forms/`) and the SPA (`/form-builder/`)
