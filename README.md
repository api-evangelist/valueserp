# ValueSERP

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

ValueSERP is a real-time Google Search API providing SERP results, image search, news search, shopping results, places, and local pack data via a simple REST interface with JSON output. Operated by Traject Data, it offers low-cost, high-reliability SERP data with no queues, batch processing capabilities, and flexible pay-as-you-go or subscription pricing.

## API Overview

The ValueSERP API base URL is `https://api.valueserp.com` and uses API key authentication via the `api_key` query parameter. All responses are returned as JSON.

### Endpoints

| Endpoint | Description |
|----------|-------------|
| `/search` | Google organic search results, ads, knowledge graphs, AI overviews |
| `/places` | Google Maps and local pack location data |
| `/shopping` | Google Shopping product listings |
| `/news` | Google News search results |
| `/product` | Product detail by product ID |
| `/place_reviews` | Location reviews from Google Maps |

### Authentication

Pass your API key as a query parameter:

```
https://api.valueserp.com/search?api_key=YOUR_API_KEY&q=pizza
```

### Output Formats

- JSON (default)
- HTML
- CSV

## Links

- **Website**: https://trajectdata.com/serp/value-serp-api/
- **Documentation**: https://docs.trajectdata.com/valueserp
- **Pricing**: https://trajectdata.com/serp/value-serp-api/pricing/
- **Status Page**: https://valueserp.statuspage.io/
- **Blog**: https://trajectdata.com/blog/
- **X**: https://x.com/valueserp
- **LinkedIn**: https://www.linkedin.com/company/traject-data/
- **Sign Up**: https://app.valueserp.com/signup

## Pricing Summary

| Plan | Searches/Month | Price/Month | Rate Limit |
|------|---------------|-------------|------------|
| Pay As You Go | Variable | $2.50/1K | 250/min |
| Starter | 25,000 | $50 | 250/min |
| Growth | 50,000 | $94 | 250/min |
| Professional | 100,000 | $158 | 500/min |
| Business | 200,000 | $240 | 1,000/min |
| Scale 500K | 500,000 | $575 | 1,250/min |
| Scale 1M | 1,000,000 | $1,000 | 1,500/min |
| Scale 5M | 5,000,000 | $3,500 | 2,000/min |
| Scale 10M | 10,000,000 | $6,000 | 5,000/min |
| Scale 20M | 20,000,000 | $10,000 | 5,000/min |

Prices shown are billed annually (10% discount). Free trial includes 100 searches with no credit card required.

## Repository Structure

```
valueserp/
  apis.yml                              # APIs.json 0.19 index
  README.md                             # This file
  plans/
    valueserp-plans-pricing.yml         # API Commons Plans 0.1
  rate-limits/
    valueserp-rate-limits.yml           # API Commons Rate Limits 0.1
  finops/
    valueserp-finops.yml                # FinOps Framework 1.0 FOCUS-aligned
```

## Maintainer

**Kin Lane** — kin@apievangelist.com
