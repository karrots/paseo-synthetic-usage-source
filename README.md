# Synthetic usage for Paseo

A Paseo usage source for Synthetic subscriptions. It reports quota data from Synthetic's [`GET /v2/quotas` endpoint](https://dev.synthetic.new/docs/synthetic/quotas).

## Install

```sh
paseo plugin install github:karrots/paseo-synthetic-usage-source
```

Requires Paseo 0.11.0 or later.

## Credentials

The source checks `PASEO_SYNTHETIC_API_KEY`, then `SYNTHETIC_API_KEY`, in the Paseo daemon environment. If neither variable is set, it looks for Synthetic credentials in OpenCode's local credential store. Environment variables take precedence.

For OpenCode credentials, the plugin reads `opencode.db` using Node's `node:sqlite` API when available and supports the legacy `auth.json` store. The OpenCode database is opened read-only. If the daemon runtime cannot load `node:sqlite`, set `PASEO_SYNTHETIC_API_KEY` in the daemon environment as a workaround.

## Usage data

The source uses the current `rollingFiveHourLimit` and `weeklyTokenLimit` fields, along with Synthetic's optional hourly search and free tool-call quotas. It accepts the legacy `subscription` quota as a five-hour fallback. Synthetic may return an empty object when an account has no quota data; Paseo displays an unavailable state for that response.

Only quota requests are sent to Synthetic. The API key is sent in the `Authorization: Bearer` header and is never included in the usage report or account identifier. Synthetic describes this API as under development, and an account without quota data can return an empty response.
