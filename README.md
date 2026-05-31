<div align="center">

<img src="assets/nawa-icon.png" width="120" alt="NAWA" />

# NAWA API · Edge Proxy

### The edge that turns `api.trynawa.com/v1/*` into NAWA's Supabase Edge Functions.

Single-purpose Vercel deployment that fronts the NAWA public API. Stable, branded URL, proper CORS headers, request-ID propagation, and zero business logic. The backend is Supabase Edge Functions; this is the door with the NAWA sign on it.

<br/>

![NAWA Green](https://img.shields.io/badge/NAWA-059669?style=flat-square)
![Vercel](https://img.shields.io/badge/host-Vercel-000000?logo=vercel&logoColor=white&style=flat-square)
![Routes](https://img.shields.io/badge/v1%20routes-15-059669?style=flat-square)
![Docs](https://img.shields.io/badge/docs-developers.trynawa.com-0891b2?style=flat-square)

</div>

---

## What it is

A `vercel.json` of rewrites and a one-page `index.html`. That is it. Every request to `api.trynawa.com/v1/*` is rewritten 1-to-1 onto the matching `xemcegxkfkvkgmehllix.supabase.co/functions/v1/api-v1-*` endpoint. Vercel handles TLS, edge caching, and a clean branded hostname; Supabase Edge Functions do the work.

## Why this exists

- **Branded, stable URL.** API consumers integrate against `api.trynawa.com`, not against a raw Supabase project ID that may change.
- **CORS, in one place.** Allowed origins, methods, headers and exposed response headers are centralised in `vercel.json` instead of repeated in every edge function.
- **A real `X-NAWA-*` response surface.** Rate-limit, cache, provider, environment, balance and calibration-version headers are all forwarded back to the client.
- **Zero business logic.** Nothing to update here except routes when a new endpoint ships.

## Routes

All routes are forwarded to a matching `api-v1-*` Supabase Edge Function.

| Route | Purpose |
|---|---|
| `POST /v1/classify` | Comment intent classification |
| `POST /v1/translate` | Translation |
| `POST /v1/detect` | Language + dialect detection |
| `POST /v1/moderate` | Moderation |
| `POST /v1/rubric/classify` | Rubric-based classification |
| `POST /v1/comments` | Comment ingest / read |
| `POST /v1/comments/{id}/reply` | Reply generation |
| `POST /v1/feedback` | RLHF feedback signal |
| `POST /v1/calibration` | Calibration data point |
| `GET  /v1/usage` | Usage + balance |
| `GET  /v1/analytics` | Analytics rollups |
| `GET  /v1/health` | Health probe |
| `GET  /v1/keys` | API key management |
| `POST /v1/webhooks` | Webhook subscription management |
| `POST /v1/stripe/webhook` | Stripe billing webhook |

Full API documentation: **[developers.trynawa.com](https://developers.trynawa.com)** (the `sunnybhara/docs` repo, deployed on Mintlify).

## Response headers exposed to clients

```
X-Request-Id                · request correlation
X-RateLimit-Limit           · current ceiling
X-RateLimit-Remaining       · what is left in this window
X-RateLimit-Reset           · seconds to reset
X-NAWA-Cache                · hit | miss | bypass
X-NAWA-Provider             · underlying model provider
X-NAWA-Environment          · prod | staging
X-NAWA-Balance              · current account balance
X-NAWA-Calibration-Version  · calibration model in use
```

## Deploy

```bash
npm install -g vercel
vercel link
vercel --prod
```

Domain (`api.trynawa.com`) is bound in the Vercel project settings.

## Files

```
.
├── vercel.json   # rewrites + CORS
├── index.html    # the page at the root
└── README.md
```

That is the whole repo. Anything more would mean the proxy started doing work it should not be doing.

---

<div align="center">
<sub>Part of the NAWA platform. Source: <a href="https://github.com/sunnybhara/docs">docs</a>, <a href="https://trynawa.com">trynawa.com</a>.</sub>
</div>
