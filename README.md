# ValueSERP

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
