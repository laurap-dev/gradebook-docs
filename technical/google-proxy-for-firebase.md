# Google-hosted proxy for Firebase from Apps Script

Future implementation note: this is a proposal doc and can be split to a dedicated PR later.

## Goal
Use a Google-hosted HTTPS proxy (Cloud Run or Cloud Functions) so Apps Script can reach Firebase (Firestore or other Google APIs) without relying on legacy RTDB secrets. The proxy enforces a shared-secret header (or HMAC/JWT) so only our Apps Script can call it.

## Overview of the flow
1) Apps Script stores a secret in Script Properties.
2) Apps Script calls the proxy HTTPS endpoint with the secret in a custom header and HMAC timestamp for replay protection.
3) The proxy validates the header/signature. If valid, it performs the desired Firebase/API call with its own service account credentials.
4) Proxy returns the result to Apps Script.

## Prerequisites
- Google Cloud project that owns the Firebase project (or has access to it).
- Billing enabled for Cloud Run / Functions.
- `gcloud` installed and authenticated locally (or use Cloud Shell).
- Service account with minimal roles: typically `Cloud Run Invoker` (for testing), plus whatever downstream API perms you need (e.g., `Firebase Realtime Database Viewer/Editor` or `Datastore User` for Firestore REST). Avoid Owner/Editor.

## Steps (Cloud Run HTTP)
1) **Create a service account for the proxy**
   ```bash
gcloud iam service-accounts create firebase-proxy-sa \
  --display-name="Firebase Proxy SA"
```
2) **Grant minimal perms** (adjust to your target API):
   ```bash
gcloud projects add-iam-policy-binding $PROJECT_ID \
  --member="serviceAccount:firebase-proxy-sa@${PROJECT_ID}.iam.gserviceaccount.com" \
  --role="roles/datastore.user"     # Firestore REST
# or roles/firebasedatabase.admin    # RTDB REST (be cautious; admin-level)
```
3) **Build a small container** (example: Node/Express) that:
   - Checks header `X-GB-Proxy-Key` against an env var `PROXY_SHARED_KEY`.
   - Optionally checks `X-GB-Proxy-Signature` and `X-GB-Proxy-Timestamp` (HMAC over body+timestamp) to prevent replay.
   - Calls Firebase REST with the service account access token (or uses `google-auth-library` to fetch one per request).
4) **Deploy to Cloud Run** (public HTTPS, but locked by header):
   ```bash
gcloud run deploy firebase-proxy \
  --source=. \
  --region=us-central1 \
  --allow-unauthenticated \
  --service-account=firebase-proxy-sa@${PROJECT_ID}.iam.gserviceaccount.com \
  --set-env-vars=PROXY_SHARED_KEY=your-long-random-secret
```
   (If you prefer IAM instead of a static header, set `--no-allow-unauthenticated` and call with an IAM-signed JWT; see options below.)
5) **Record the HTTPS URL** from the deploy output.
6) **Store the secret in Apps Script**: Script Properties → `PROXY_SHARED_KEY = your-long-random-secret`.
7) **Call from Apps Script** (example):
   ```javascript
   function callProxy(payload) {
     var props = PropertiesService.getScriptProperties();
     var key = props.getProperty('PROXY_SHARED_KEY');
     var url = 'https://firebase-proxy-xxxxxxxx-uc.a.run.app/do-something';
     var options = {
       method: 'post',
       contentType: 'application/json',
       payload: JSON.stringify(payload),
       headers: {
         'X-GB-Proxy-Key': key
         // Optionally include X-GB-Proxy-Timestamp and X-GB-Proxy-Signature (HMAC)
       },
       muteHttpExceptions: true
     };
     var res = UrlFetchApp.fetch(url, options);
     return JSON.parse(res.getContentText());
   }
   ```

## Also include: stronger auth patterns
- **HMAC**: Proxy recomputes HMAC(secret, body + timestamp) and enforces a 5-minute window.
- **IAM/IAP**: Instead of `--allow-unauthenticated`, restrict Cloud Run and:
  - Use Apps Script to obtain an OAuth access token for a service account via the OAuth2 library, then set `Authorization: Bearer <token>`.
  - Or place IAP in front and verify the IAP JWT in the proxy (heavier, but no shared secret in headers).

## Cloud Functions instead of Cloud Run
- Similar handler code; deploy with `gcloud functions deploy ... --runtime nodejs20 --trigger-http --allow-unauthenticated --set-env-vars PROXY_SHARED_KEY=...`.
- Same header/HMAC checks apply. Cloud Run is usually preferred for better cold-starts and container flexibility.

## Why this is safe(r) than legacy RTDB secret
- The proxy never exposes Firebase secrets to the client; Apps Script only holds a scoped proxy secret.
- You can rotate `PROXY_SHARED_KEY` without touching Firebase config.
- You can layer WAF/rate limits in Cloud Run and IAM/IAP if desired.

## What to monitor/maintain
- Rotate `PROXY_SHARED_KEY` periodically; keep old key for a short overlap window if needed
- Monitor proxy logs for 401/403 spikes.
- Keep service account roles minimal; prefer read-only where possible.
- Rebuild the container when runtimes deprecate.

## Quick checklist
- [ ] Service account created
- [ ] Minimal roles granted
- [ ] Proxy deployed (Cloud Run or Functions)
- [ ] Shared key stored as env var and in Script Properties
- [ ] Apps Script call uses custom header (and optional HMAC)
- [ ] Logs/metrics monitored; key rotation process documented
