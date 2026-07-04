# Pollfish (pollfish)

Pollfish is a mobile-first survey and market-research platform owned by **Prodege LLC**. It has two sides: researchers reach real respondents inside mobile apps and websites to run surveys, and app publishers monetize their audience by serving Pollfish (and mediated third-party) surveys as rewarded ads or an offerwall. Pollfish exposes a REST **Dashboard API** on `https://www.pollfish.com` for managing publisher apps and pulling performance, revenue, demographic, and user-log analytics (HTTP Basic Auth), a **survey-serving / offerwall API** on `https://wss.pollfish.com` for requesting and rendering surveys (plain HTTPS GET/HEAD — the `wss` hostname is *not* WebSocket), and **server-to-server postback callbacks** for survey-completion and eligibility events. Survey creation and audience targeting for researchers are done in the Pollfish dashboard and are not exposed as a documented public REST API.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/pollfish/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/pollfish/refs/heads/main/apis.yml)

## Tags

- Surveys
- Market Research
- Mobile
- Monetization
- Offerwall
- Rewarded Ads
- Prodege

## Timestamps

- **Created:** 2026-07-04
- **Modified:** 2026-07-04

## APIs

### Pollfish Apps API

Create, read, update, and delete the publisher apps (monetization placements) that receive Pollfish surveys. Each app carries category, platform, behavior, reward-currency, and offerwall configuration, and returns an `api_key` used across the rest of the platform. HTTP Basic Auth with Pollfish email plus account secret key.

- **Human URL:** [https://www.pollfish.com/docs/dashboard-api](https://www.pollfish.com/docs/dashboard-api)
- **Base URL:** `https://www.pollfish.com/api/public/v2`

### Pollfish Performance API

Pull survey-serving performance metrics (served, seen, accepted, completed) per survey network, for all apps or a single app, over a date range of up to 31 days, optionally grouped by country.

- **Human URL:** [https://www.pollfish.com/docs/dashboard-api](https://www.pollfish.com/docs/dashboard-api)
- **Base URL:** `https://www.pollfish.com/api/public/v3`

### Pollfish Revenue API

Report publisher revenue per survey provider (Pollfish, Toluna, Cint, and other mediation networks) and per country, for all apps or a single app, over a date range, with optional ISO country filtering.

- **Human URL:** [https://www.pollfish.com/docs/dashboard-api](https://www.pollfish.com/docs/dashboard-api)
- **Base URL:** `https://www.pollfish.com/api/public/v3`

### Pollfish Respondent Demographics API

Fetch the collected demographic profile (gender, year of birth, education, employment, income, marital status, parental, race, career) for a given respondent device id, using Pollfish's demographic enumeration codes.

- **Human URL:** [https://www.pollfish.com/docs/demographic-surveys](https://www.pollfish.com/docs/demographic-surveys)
- **Base URL:** `https://www.pollfish.com/api/public/v3`

### Pollfish User Logs API

Retrieve paginated per-user survey logs for a given `device_id` or `request_uuid` — which surveys were started and completed, and the disqualification reason (quota full, screenout, duplicate, VPN, quality, and other public termination reasons) when not completed.

- **Human URL:** [https://www.pollfish.com/docs/dashboard-api](https://www.pollfish.com/docs/dashboard-api)
- **Base URL:** `https://www.pollfish.com/api/public/v3`

### Pollfish Survey Distribution and Offerwall API

The SDK-alternative survey-serving surface. Register a device to request a matching survey (or a JSON offerwall list of surveys with reward and remaining-completes data), then load the returned `survey_link` to render the survey to the respondent. Supports single-survey, HTML offerwall, and JSON offerwall modes, targeting via demographic and device parameters, and reward-conversion signing. The host is `wss.pollfish.com` but the transport is HTTPS GET, not WebSocket.

- **Human URL:** [https://www.pollfish.com/docs/api-documentation](https://www.pollfish.com/docs/api-documentation)
- **Base URL:** `https://wss.pollfish.com`

### Pollfish Server-to-Server Callbacks

Publisher-configured server-to-server postback callbacks. Pollfish fires an HTTP GET to a URL template you register in the dashboard on survey completion and (optionally) on user-not-eligible events, passing through `cpa`, `reward_name`, `reward_value`, `request_uuid`, `tx_id`, `click_id`, `status`, `term_reason`, and a `signature` for verification. Outbound postbacks only — there is no inbound webhook-management REST endpoint and no WebSocket.

- **Human URL:** [https://www.pollfish.com/docs/s2s](https://www.pollfish.com/docs/s2s)

## Authentication

- **Dashboard API** (`www.pollfish.com`) — HTTP Basic Auth. Username is your Pollfish account email; password is your account secret key (from Account Information in the publisher dashboard).
- **Survey-serving API** (`wss.pollfish.com`) — Your app `api_key` and `placement_key` are passed inside the URL-encoded `json` query parameter; reward-conversion parameters are protected with an HMAC-SHA1 `sig`.

## WebSocket Review

**Does Pollfish expose a documented public WebSocket API? No.** All documented surfaces are request/response over HTTPS. The `wss.pollfish.com` host is a plain HTTPS endpoint (the label is not the WebSocket Secure scheme). Pollfish's only server-initiated messaging is the one-way s2s postback (an outbound HTTP GET to the publisher's URL), which is a webhook, not a WebSocket. See [review.yml](review.yml).

## Common Properties

- [GitHub Organization](https://github.com/pollfish)
- [LinkedIn](https://www.linkedin.com/company/pollfish)
- [Website](https://www.pollfish.com)
- [Documentation](https://www.pollfish.com/docs)
- [Plans](plans/pollfish-plans-pricing.yml)
- [Rate Limits](rate-limits/pollfish-rate-limits.yml)
- [Fin Ops](finops/pollfish-finops.yml)
- [Pricing](https://www.pollfish.com/pricing/)
- [Blog](https://www.pollfish.com/blog/)

## Maintainers

**FN:** Kin Lane
**Email:** kin@apievangelist.com
