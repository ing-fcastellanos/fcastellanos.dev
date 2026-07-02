# CI setup — Firebase deploy via Workload Identity Federation (keyless)

The GitHub Actions workflows deploy to Firebase Hosting **without any
long-lived key**. They authenticate to Google Cloud with Workload Identity
Federation (WIF): GitHub mints a short-lived OIDC token, GCP trusts it for
this repo only, and lets it impersonate a deploy service account.

Run this **once**. All commands use the Google Cloud project `fcastellanos-info`.

## 0. Prerequisites

```bash
gcloud auth login
gcloud config set project fcastellanos-info

# Grab the numeric project number — used below
PROJECT_ID=fcastellanos-info
PROJECT_NUMBER=$(gcloud projects describe "$PROJECT_ID" --format='value(projectNumber)')
REPO=ing-fcastellanos/fcastellanos.dev   # update if the repo name differs

# Enable required APIs
gcloud services enable iamcredentials.googleapis.com \
  firebasehosting.googleapis.com --project "$PROJECT_ID"
```

## 1. Deploy service account

```bash
gcloud iam service-accounts create github-deployer \
  --project "$PROJECT_ID" \
  --display-name "GitHub Actions – Firebase Hosting deployer"

SA_EMAIL="github-deployer@${PROJECT_ID}.iam.gserviceaccount.com"

# Minimum role needed to deploy hosting
gcloud projects add-iam-policy-binding "$PROJECT_ID" \
  --member="serviceAccount:${SA_EMAIL}" \
  --role="roles/firebasehosting.admin"
```

## 2. Workload Identity Pool + GitHub provider

```bash
gcloud iam workload-identity-pools create github-pool \
  --project "$PROJECT_ID" --location=global \
  --display-name "GitHub Actions pool"

gcloud iam workload-identity-pools providers create-oidc github-provider \
  --project "$PROJECT_ID" --location=global \
  --workload-identity-pool=github-pool \
  --display-name "GitHub OIDC" \
  --issuer-uri="https://token.actions.githubusercontent.com" \
  --attribute-mapping="google.subject=assertion.sub,attribute.repository=assertion.repository" \
  --attribute-condition="assertion.repository=='${REPO}'"
```

## 3. Let the repo impersonate the service account

```bash
gcloud iam service-accounts add-iam-policy-binding "$SA_EMAIL" \
  --project "$PROJECT_ID" \
  --role="roles/iam.workloadIdentityUser" \
  --member="principalSet://iam.googleapis.com/projects/${PROJECT_NUMBER}/locations/global/workloadIdentityPools/github-pool/attribute.repository/${REPO}"
```

## 4. Wire the values into GitHub

The workflows read two **repository variables** (not secrets — these values
are not sensitive):

```bash
# Full provider resource name
echo "projects/${PROJECT_NUMBER}/locations/global/workloadIdentityPools/github-pool/providers/github-provider"
# Service account email
echo "$SA_EMAIL"
```

In the repo → **Settings → Secrets and variables → Actions → Variables**, add:

| Name | Value |
|------|-------|
| `WIF_PROVIDER` | the provider resource name above |
| `WIF_SERVICE_ACCOUNT` | `github-deployer@fcastellanos-info.iam.gserviceaccount.com` |

## 5. Clean up the old secret

Once a deploy succeeds, delete the legacy JSON secret
`FIREBASE_SERVICE_ACCOUNT_FRANKPERSONALPROJECT` from
**Settings → Secrets and variables → Actions → Secrets**.

---

> **Note:** the `attribute-condition` and the `principalSet` binding both pin
> access to `ing-fcastellanos/fcastellanos.dev`. If you rename the repo, update
> `REPO` and re-run steps 2–3, and update the `WIF_PROVIDER`/binding accordingly.
