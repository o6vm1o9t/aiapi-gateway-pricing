# gateway API (ai api gateway) — PRICING guide with per-unit pricing

> **one key, published unit prices** — image2.5 from $0.0085/image, video from $0.01056/second, LLM input from $0.0228 per million tokens.

**[Model page](https://go.apimart.ai/k-658281)** · **[Live pricing](https://go.apimart.ai/k-f27bba)** · **[Get an API key](https://go.apimart.ai/k-4718da)**

Everything on this page refers to **gateway** — also written **ai api gateway**, **gateway** or **gateway** — served through the OpenAI-compatible APIMart gateway at `https://api.apimart.ai/v1`.

## Pricing (observed, snapshot 2026-09-24)

| Tier | Price |
| --- | --- |
| `GPT Image 2.5 (1K)` | $0.0085 |
| `Seedance 2.0 Mini (480P, per sec)` | $0.0106 |
| `Qwen3.7 Flash (per M input)` | $0.0229 |

Billed per unit, pay as you go, **$1 minimum top-up**, no subscription and no free quota. Every task response returns `cost` / `credits_cost` so the charge can be checked per call.

## What that costs at scale

| Volume | Cost |
| --- | --- |
| 100 | $2.2857 |
| 1000 | $22.8568 |

Linear at the observed rate, no volume discount assumed.

## How to call it

```bash
export APIMART_API_KEY="<token>"
curl --request POST --url https://api.apimart.ai/v1/images/generations \
  --header "Authorization: Bearer $APIMART_API_KEY" --header 'Content-Type: application/json' \
  --data '{"model":"gateway","prompt":"a modern cliffside villa at dusk, slow camera push","size":"16:9","n":1}'
```

Submit, keep the `task_id`, then poll `GET /v1/tasks/{id}` until `completed`. The response carries the image/video URL and the exact amount charged.

## Troubleshooting the first call

| Symptom | Cause | Fix |
| --- | --- | --- |
| `401` | key missing or truncated | re-copy from the console; header is `Authorization: Bearer $APIMART_API_KEY` |
| no balance | account has no credit | top up from $1 — there is no free tier on any model here |
| `429` | too many concurrent calls on one key | back off, retry with the same `Idempotency-Key` |
| `model` not found | wrong id or wrong tier field | copy the exact id `gateway` (alias `ai api gateway`) from the table above |
| task `failed` | prompt filtered, or a reference URL expired | resubmit with a new `Idempotency-Key` |

## FAQ

**How is it billed?** Per unit of usage; published rates above.
**Which resolutions?** GPT Image 2.5 (1K), Seedance 2.0 Mini (480P, per sec), Qwen3.7 Flash (per M input).
**Is this a relay?** Yes — APIMart is a third-party gateway. Same OpenAI-compatible request shape, different billing and settlement from the vendor's direct API.

## Disclosure

This repository documents access through APIMart, a third-party API gateway, and is not affiliated with the model vendor. Prices are the dated snapshot above; the platform console is authoritative for billing.
