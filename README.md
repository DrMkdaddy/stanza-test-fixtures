# @stanza-api/test-fixtures

Engine-validated sample payloads for ANSI X12, ISO 20022, and GS1 parsers — for unit tests, CI fixtures, sandbox seeding, and parser development.

Every fixture is parsed by the production Stanza API engines in this repository's test suite before it ships: X12 payloads pass envelope and SNIP checks, ISO 20022 messages are validated and parsed, and the GS1 element string is decoded end to end. If a fixture stops parsing, the build fails.

## Install

```bash
npm install @stanza-api/test-fixtures
```

Zero dependencies. ESM, typed.

## Usage

```ts
import { FIXTURES, FIXTURES_BY_SLUG } from '@stanza-api/test-fixtures';

// Seed a test database or a fixture file
const claim835 = FIXTURES_BY_SLUG['835-remittance-advice'];
console.log(claim835.filename); // '835-remittance-advice.edi'
console.log(claim835.mediaType); // 'text/plain'
console.log(claim835.content); // the raw EDI payload

// Iterate everything by standard
for (const fixture of FIXTURES) {
  console.log(fixture.standard, fixture.slug, fixture.content.length);
}
```

Each fixture exposes:

| Field | Description |
| --- | --- |
| `slug` | Stable identifier (`835-remittance-advice`) |
| `title` | Human-readable name |
| `standard` | `ANSI X12 5010`, `ISO 20022`, or `GS1` |
| `transactionType` | `837P`, `837I`, `835`, `834`, `270`, `pacs.008`, `camt.053`, `camt.054`, `pain.001`, `GS1` |
| `description` | What the payload contains |
| `pagePath` | The Stanza spec page documenting the transaction |
| `filename` | Suggested download filename |
| `mediaType` | `text/plain` or `application/xml` |
| `content` | The raw payload |

Also exported: `FIXTURES_BY_SLUG` and `FIXTURES_BY_PAGE` lookup maps.

## Fixtures

| Slug | Standard | Transaction |
| --- | --- | --- |
| `837p-professional-claim` | ANSI X12 5010 | 837P professional claim |
| `837i-institutional-claim` | ANSI X12 5010 | 837I institutional claim |
| `835-remittance-advice` | ANSI X12 5010 | 835 remittance advice |
| `270-eligibility-inquiry` | ANSI X12 5010 | 270 eligibility inquiry |
| `271-eligibility-response` | ANSI X12 5010 | 271 eligibility response |
| `276-claim-status-inquiry` | ANSI X12 5010 | 276 claim status inquiry |
| `277-claim-status-response` | ANSI X12 5010 | 277 claim status response |
| `834-benefit-enrollment` | ANSI X12 5010 | 834 benefit enrollment |
| `pacs-008-credit-transfer` | ISO 20022 | pacs.008 credit transfer |
| `pacs-002-payment-status` | ISO 20022 | pacs.002 payment status report |
| `camt-053-bank-statement` | ISO 20022 | camt.053 bank statement |
| `camt-054-debit-credit-notification` | ISO 20022 | camt.054 debit/credit notification |
| `pain-001-payment-initiation` | ISO 20022 | pain.001 payment initiation |
| `pain-002-payment-status-report` | ISO 20022 | pain.002 payment status report |
| `gs1-element-string` | GS1 | GTIN + lot + expiry + serial element string |
| `sscc-element-string` | GS1 | SSCC (AI 00) element string |
| `gs1-digital-link-uri` | GS1 | GS1 Digital Link URI |

The same files are downloadable from https://stanzaapi.com/samples, and a machine-readable index is served at https://stanzaapi.com/samples.json.

## Notes

- All fixtures are synthetic test data. They contain no real patient, payer, account holder, or company information.
- Never use test fixtures in live transactions.
- Payloads are intentionally minimal but structurally complete; edit identifiers, amounts, and dates for your scenario and re-validate with the matching Stanza parser.
