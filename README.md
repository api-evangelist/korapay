# Kora (korapay)

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

Kora (formerly Korapay) is a pan-African payments infrastructure company that lets businesses collect payments, disburse payouts, run settlements, issue cards, verify identities, and check balances across African markets. The Kora merchant REST API (base `https://api.korapay.com/merchant/api/v1`) supports pay-ins via card, bank transfer, mobile money, and pay-with-bank; single, bulk, and remittance payouts; NGN and USD virtual bank accounts; balances; refunds; and multi-currency conversion.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/korapay/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/korapay/refs/heads/main/apis.yml)

## Access Model (Honest Summary)

- **REST + webhooks only.** The public surface is a request/response REST API over HTTPS at `https://api.korapay.com/merchant/api/v1`. Asynchronous transaction updates are delivered as **webhooks** (server-to-your-endpoint HTTP POST). There is **no documented public WebSocket (`wss://`) API**.
- **Two key types, two modes.** Every account has a **public key** (`pk_...`, non-sensitive, client-side, can only initiate transactions) and a **secret key** (`sk_...`, server-side, required for any financial data or account operation). Keys differ between **Test** and **Live** modes. The secret key is passed as `Authorization: Bearer sk_...`.
- **Card charges are encrypted.** Card payment payloads must be **AES-256 encrypted** with your account **encryption key** and sent as a single `charge_data` field. Card payments also require the feature to be enabled on your account and **PCI DSS Level 1** certification.
- **Webhook authenticity.** Webhook requests carry an `x-korapay-signature` header, an **HMAC SHA-256** of only the `data` object of the payload, signed with your secret key. Verify it before delivering value.
- Some products (card payments, mobile money) must be explicitly enabled on your account by Kora support before use.

## Tags

- Payments
- Payment Gateway
- Africa
- Nigeria
- Collections
- Payouts
- Disbursements
- Virtual Bank Account
- Cards
- Fintech

## Timestamps

- **Created:** 2026-07-12
- **Modified:** 2026-07-12

## APIs

### Kora Charges API

Accept payments (pay-ins) from customers. Initialize a checkout charge, charge cards (encrypted, with OTP/PIN/AVS/phone authorization), charge and authorize mobile money wallets, run pay-with-bank (Instant EFT / direct debit) charges, and verify the final status of any charge by reference.

- **Human URL:** [https://developers.korapay.com/docs/accept-payments](https://developers.korapay.com/docs/accept-payments)
- **Base URL:** `https://api.korapay.com/merchant/api/v1`

#### Tags

- Collections
- Charges
- Cards
- Bank Transfer
- Mobile Money

#### Properties

- [Documentation](https://developers.korapay.com/docs/accept-payments)
- [API Reference](https://developers.korapay.com/docs/accepting-card-payments-with-apis)
- [OpenAPI](openapi/korapay-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/korapay.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/korapay.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### Kora Payouts API

Disburse funds from your Kora balance to bank accounts and mobile money wallets. Send single payouts, bulk payouts (batches of 2+), and cross-border remittance payouts, check payout availability, and fetch your payout history.

- **Human URL:** [https://developers.korapay.com/docs/payout-via-api](https://developers.korapay.com/docs/payout-via-api)
- **Base URL:** `https://api.korapay.com/merchant/api/v1`

#### Tags

- Payouts
- Disbursements
- Transfers
- Bulk

#### Properties

- [Documentation](https://developers.korapay.com/docs/payout-via-api)
- [API Reference](https://developers.korapay.com/docs/bulk-payouts-via-api)
- [OpenAPI](openapi/korapay-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/korapay.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/korapay.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### Kora Virtual Bank Accounts API

Create and manage dedicated NGN and USD virtual bank accounts (permanent or temporary) tied to your customers, retrieve an account by its reference, and fetch the transactions that funded a virtual bank account.

- **Human URL:** [https://developers.korapay.com/docs/virtual-bank-accounts](https://developers.korapay.com/docs/virtual-bank-accounts)
- **Base URL:** `https://api.korapay.com/merchant/api/v1`

#### Tags

- Virtual Bank Account
- Collections
- Nigeria

#### Properties

- [Documentation](https://developers.korapay.com/docs/virtual-bank-accounts)
- [API Reference](https://developers.korapay.com/docs/virtual-bank-accounts-ngn)
- [OpenAPI](openapi/korapay-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/korapay.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/korapay.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### Kora Balances API

Retrieve real-time available and pending balances across all supported currencies (NGN by default, plus USD, GHS, KES, XAF, XOF, ZAR for multi-currency accounts). Requires secret key authentication.

- **Human URL:** [https://developers.korapay.com/docs/balance-api](https://developers.korapay.com/docs/balance-api)
- **Base URL:** `https://api.korapay.com/merchant/api/v1`

#### Tags

- Balance
- Reporting
- Multi-Currency

#### Properties

- [Documentation](https://developers.korapay.com/docs/balance-api)
- [OpenAPI](openapi/korapay-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/korapay.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/korapay.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### Kora Refunds API

Initiate a full or partial refund for a completed pay-in transaction, retrieve the details of a single refund by reference, and list refunds within a given time period.

- **Human URL:** [https://developers.korapay.com/docs/refunds-api](https://developers.korapay.com/docs/refunds-api)
- **Base URL:** `https://api.korapay.com/merchant/api/v1`

#### Tags

- Refunds
- Collections

#### Properties

- [Documentation](https://developers.korapay.com/docs/refunds-api)
- [OpenAPI](openapi/korapay-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/korapay.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/korapay.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### Kora Currency Conversion API

Get the latest exchange rate for a currency pair, initiate a currency conversion between your multi-currency balances, retrieve a conversion by reference, and list conversion history for a period.

- **Human URL:** [https://developers.korapay.com/docs/currency-conversion-api](https://developers.korapay.com/docs/currency-conversion-api)
- **Base URL:** `https://api.korapay.com/merchant/api/v1`

#### Tags

- Currency Conversion
- FX
- Multi-Currency

#### Properties

- [Documentation](https://developers.korapay.com/docs/currency-conversion-api)
- [API Reference](https://developers.korapay.com/docs/exchange-rate-api)
- [OpenAPI](openapi/korapay-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/korapay.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/korapay.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### Kora Misc Utilities API

Payout support utilities - list banks and mobile money operators by country, resolve (verify) a destination bank account or mobile money number, and look up payout payment purposes and supported payout countries by currency before initiating a payout.

- **Human URL:** [https://developers.korapay.com/docs/payout-utilities](https://developers.korapay.com/docs/payout-utilities)
- **Base URL:** `https://api.korapay.com/merchant/api/v1`

#### Tags

- Banks
- Account Resolution
- Utilities

#### Properties

- [Documentation](https://developers.korapay.com/docs/payout-utilities)
- [OpenAPI](openapi/korapay-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/korapay.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/korapay.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

## Common Properties

- [Domain Security](security/korapay-domain-security.yml)
- [Authentication](authentication/korapay-authentication.yml)
- [GitHub Organization](https://github.com/korapay)
- [LinkedIn](https://www.linkedin.com/company/korapay)
- [Website](https://www.korahq.com)
- [Documentation](https://developers.korapay.com/docs/getting-started)
- [Plans](plans/korapay-plans-pricing.yml)
- [Rate Limits](rate-limits/korapay-rate-limits.yml)
- [Fin Ops](finops/korapay-finops.yml)

## Maintainers

**FN:** Kin Lane
**Email:** kin@apievangelist.com
