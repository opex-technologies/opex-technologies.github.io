# Opex Technologies

## Project Overview
Form builder application with authentication, scoring engine, and PDF export. SPA frontend deployed to GitHub Pages with Cloud Function backends on GCP.

## Tech Stack
- **Frontend**: React/TypeScript (Vite) → GitHub Pages
- **Backend**: Python Cloud Functions (GCP)
- **Database**: Firestore (email-keyed user management)
- **GCP Project**: `opex-data-lake-k23k4y98m`

## Key URLs
- **UI**: https://opex-technologies.github.io
- **APIs**: `us-central1-opex-data-lake-k23k4y98m.cloudfunctions.net`

## Cloud Functions
- `auth-api` — User authentication (Firestore-backed, account lockout)
- `form-builder-api` — Form CRUD operations
- `response-scorer-api` — Response scoring engine

## Deployment
- Frontend: Push to `main` → GitHub Pages auto-deploy
- Functions: `gcloud functions deploy <name> --project opex-data-lake-k23k4y98m --region us-central1`

## Key Contacts
- **Adam Trzonkowski** — VP Solutions Engineering (primary)
- **Ben Thornton** — Domain/DNS
- **Ken Nowell** — Technical

## Repositories
- **This repo** (`opex-technologies/opex-technologies.github.io`) — Frontend SPA (GitHub Pages)
- **Backend repo** (`~/Documents/opex-technologies-src`) — Cloud Function source code (`landoncolvig/opex-technologies`)

## Notes
- SPA routing uses 404.html redirect pattern
- Account lockout uses datetime comparison (known previous bug area)
