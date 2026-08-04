# Pollfish (pollfish)

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

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
