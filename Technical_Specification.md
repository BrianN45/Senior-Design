# BarCat Deals: Technical Specification

**Course:** CS 5001 - Senior Design I, University of Cincinnati  
**Team:** Brian Nguyen, Kaustubh Mathur, Ved Sanap, Meredith Bartel, Kiki Vasilev  
**Advisor:** Dr. Hrishikesh Vinayak Bhide  
**Version:** 0.3 | **Date:** October 28, 2026 | **Status:** Draft for review

**Requirements baseline:** [Requirements Document](Requirements_Document.md), **version 0.2**, dated **October 28, 2026**. Section 6 traces every REQ, NFR, and CON ID to this version. Its [glossary](Requirements_Document.md#4-glossary) and [assumptions/open-question register](Requirements_Document.md#9-assumptions-and-open-questions) also apply to this specification.

## 1. Version and Change Log

| Version | Date | Change | Author |
| --- | --- | --- | --- |
| 0.1 | 2026-10-28 | Consolidated D0-D2 design and requirement mappings. | AI-assisted team draft |
| 0.2 | 2026-10-28 | Shortened explanations and tables; preserved interfaces, constraints, and complete traceability. | AI-assisted revision |
| 0.3 | 2026-10-28 | Referenced Requirements Document version 0.2 and its shared assumptions/open-question register. | AI-assisted revision |


## 2. Architecture Summary

BarCat Deals uses a client-server architecture with background ingestion. **C1 Student Web App** and **C2 Owner Web Portal** render Flask/Jinja pages and call **C3 Backend API Service** over HTTPS/JSON. C3 validates requests, applies age and geographic eligibility, and controls all application access to **C4 Deal Database**, a managed PostgreSQL database.

For browsing, C3 constructs scheduled occurrences in Eastern time and returns eligible deals with source and freshness information. The default feed includes today's offers, including those starting later today. For owner submissions, C3 checks the approved owner-to-venue mapping and commits the deal and submission result before returning success.

**C5 Deal Ingestion Service** web scrapes approved venue social-media pages from **C6 External Social Media Sources**, asks an AI model for structured deals, and submits validated results through C3. Posts, processing leases, and results remain durable so failed work can recover. External requests and AI calls run outside database transactions and the browsing request path. OQ-01 defines source approval, request limits, and funding controls for web scraping. The linked D0-D2 diagrams retain the historical C6 label "External Social Media APIs".

Deployment uses separate Flask, API, and worker containers on Azure with managed PostgreSQL. The exact Azure service and budget remain unconfirmed. Shared modules **G1 authentication**, **G2 inline notices**, **G3 configuration**, and **G4 logging/health** run within these components. Supabase Auth supplies verified owner identity; students can browse through guest sessions. C5 receives no database credentials.

Existing diagrams are linked rather than duplicated:

- [D0 block diagram](Design_Diagrams/D0_Detailed_Design.md#block-diagram)
- [D0 data-flow diagram](Design_Diagrams/D0_Detailed_Design.md#data-flow-diagram)
- [D1 entity-relationship diagram](Design_Diagrams/D1_Detailed_Design.md#21-entity-relationship-diagram)
- [D2 student browsing and owner submission diagram](Design_Diagrams/D2_Detailed_Design.md#22-c1-student-web-app)
- [D2 relational additions diagram](Design_Diagrams/D2_Detailed_Design.md#24-c4-data-additions-and-invariants)
- [D2 ingestion states and failure paths diagram](Design_Diagrams/D2_Detailed_Design.md#27-c5-ingestion-and-c6-adapters)

## 3. Interfaces and Data Formats

### 3.1 Component Boundaries

| Boundary | Inputs and outputs / format | Protocol and failure handling |
| --- | --- | --- |
| Browser to C1/C2 | Filters, cookie, form/CSRF values; HTML pages and notices | HTTPS; preserve invalid form input, identify field errors, and distinguish failed requests from empty results. |
| I1: C1 to C3 | Feed/detail queries and session token; deal JSON | HTTPS; C3 applies eligibility; C1 displays typed errors and hides alcohol after failed revalidation. |
| I2: C2 to C3 | Owner input, session, request key; committed deal ID | HTTPS JSON; 422 validation, 403 authority, 409 conflict, 503 dependency failure. |
| I3: C5 to C3 | Posts, claims, completions, budget requests; durable result JSON | Restricted HTTPS with service key; 401 bad credential, 409 stale lease, 503 retryable failure. |
| I4: C3 to C4 | Parameterized SQL/transactions; rows and commit results | PostgreSQL connection through SQLAlchemy; rollback on failure, retry one deadlock, otherwise return 503. |
| I5: C5 to C6 | Approved venue page URL/account and pagination limit; HTML, post text, images, and timestamps normalized by C5 into post records | HTTPS web scraping; honor request limits, stop on denied access, retry transient failures, and quarantine invalid content. |
| C5 to AI | Post text/images/date; 0-10 structured candidate deals | Anthropic HTTPS API; 30-second timeout, schema validation, bounded retries. |
| G1 to identity provider | Access token; verified subject and expiry | Supported Supabase verification interface; 401 invalid identity, 503 unavailable verification. |

All API paths below use `/api/v1`, except health endpoints. Errors contain `error:{code,message,fields}` and `request_id`; `fields` maps field names to message arrays. Common codes are 400 malformed JSON, 401 invalid authentication, 403 forbidden, 404 unavailable resource, 409 conflict, 422 invalid fields, 429 rate limit, and 503 unavailable dependency. Responses and logs omit secrets; deal/session responses use `Cache-Control: no-store`.

### 3.2 Public API

| Method and path | Input | Success / specific failure |
| --- | --- | --- |
| GET `/deals` | Mode, date/instant, venue/category, alcohol flag, sort, limit, cursor | 200 feed; 403 unauthorized alcohol request; 422 invalid filter/cursor. |
| GET `/deals/{id}` | Deal ID, optional session | 200 detail; 403 age restriction; 404 missing, expired, unpublished, or out of radius. |
| POST `/owner/deals` | Owner session, UUID idempotency key, owner input | 201 published ID/time after commit; replay exact retry; 409 conflict; 422 field/profile error. |
| POST `/sessions/guest` | Empty JSON | 201 session/CSRF tokens, expiry, age=false; 429 abuse limit. |
| PATCH `/session/age` | Session, `confirmed_21_plus:bool` | 200 age result; false removes eligibility; 401 expired session. |
| POST `/sessions/login` | Verified provider bearer token | 201 session, role, expiry, age; 403 unapproved owner. |
| DELETE `/session` | Session | 204 persisted revocation; 503 if persistence fails. |
| GET `/health/live`; GET `/health/ready` | None; no API prefix | Liveness 200; readiness 200 only with required settings/database, otherwise 503. |

Today mode defaults to the current Eastern date; active mode defaults to now. Both permit queries through 14 days ahead. Reject date/instant parameters in the wrong mode. Categories are food/drink/other; alcohol defaults false; limit is 1-50, default 20. Today sorts by occurrence start/ID; active defaults to food, remaining minutes, then ID. Ending-soon sorting requires active mode.

### 3.3 Data Contracts

IDs are opaque strings of 1-128 characters. Stored instants use UTC; schedules use `America/New_York`, dates `YYYY-MM-DD`, times `HH:MM`, and distinct weekdays 1-7. Intervals include their start and exclude their end.

| Record | Fields and validation |
| --- | --- |
| Owner input | Required business name (1-160), address (1-300), contact email (valid, <=254), description (1-1,000), category, alcohol boolean, weekdays, unequal start/end times. These represent seven logical form items. Optional title/price (1-120), effective start date, nullable end date up to one year ahead. Venue authority comes from the owner mapping. |
| DealView | ID; venue ID/name/address; title/description; category/alcohol/source; weekdays/times/effective dates; price text; distance in meters; source URL; last-check timestamp/stale flag; occurrence start/end; nullable remaining minutes. Excludes owner email and worker/session internals. |
| FeedResponse | DealView array, nullable next cursor, generation timestamp, nullable last-update timestamp, stale count. Summary values cover all matching records. Empty success has an empty array, null cursor/update, and count 0. |
| Normalized post | Platform/account/external IDs, text <=5,000 characters, 0-10 image URLs, posted/observed UTC timestamps, HTTPS source URL. PostPage adds a nullable next cursor; fetch limit is 1-100, default 25. |
| Extracted deal | Title <=120, description <=1,000, category, alcohol, schedule/effective dates, price, source URL, confidence 0-1. Maximum 10 per post; automated dates follow D1's 60-day horizon. |

C4 retains D1's VENUE, BUSINESS_OWNER, BASE_DEAL, SUBMITTED_DEAL, and AUTOMATED_DEAL relationships. Each base deal has exactly one provenance subtype. D2 adds:

- **SOCIAL_ACCOUNT / SOCIAL_MEDIA_POST:** account and post uniqueness by platform/external ID; durable cursor, retry, lease, and result fields.
- **DEAL_SOURCE:** composite `(deal_id,post_id)` key preserving merged source evidence.
- **WEB_SESSION:** token hash, identity/role, age, CSRF hash, expiry/revocation.
- **REQUEST_RESULT:** `(owner_id,request_key)`, body hash and original response, retained 24 hours.
- **BUDGET_RESERVATION:** unique attempt ID, month, and reserved cost, checked atomically against the cap.

Foreign keys, unique constraints, and atomic transactions prevent partial publication. Index joins, published venue records, due work, and expiries. C3 controls migrations and queue claims. Bookmarks and SAVED_DEAL remain deferred. Full relational fields and invariants remain in D1 and D2 Section 2.4.

### 3.4 Internal Ingestion API

The following paths add `/internal` after `/api/v1`. Every operation requires the worker service key; 401/422/503 are common failures.

| POST path | Input | Result / recovery |
| --- | --- | --- |
| `/posts` | Account ID, <=100 normalized posts | Per-post ID and inserted/duplicate/quarantined result. |
| `/accounts/{id}/poll-claim` | Worker ID | Token, cursor, expiry; 204 if busy/not due. |
| `/accounts/{id}/poll-heartbeat` | Poll token | Extended expiry; 409 stale token. |
| `/accounts/{id}/poll-complete` | Token, cursor, optional error/backoff | Poll state; advance cursor only after durable registration. |
| `/posts/claim` | Worker ID | Post, lease token, expiry, attempt; 204 no work. |
| `/posts/{id}/heartbeat` | Lease token | Extended expiry; 409 lost lease. |
| `/automated-deals` | Post/token, 0-10 extracted deals | Per-index disposition/ID; replay stored completion; 409 stale lease. |
| `/posts/{id}/failure` | Token, typed error, retryable flag | Status/next attempt; server enforces attempt limit. |
| `/budget/reservations` | Attempt/post IDs, lease token | Allowed flag/reservation ID; denial pauses AI without consuming an attempt. |

Poll approved accounts every 15 minutes or the longer provider interval. Leases last two minutes, extended by 30-second heartbeats. Permit three processing attempts, one AI call each; retry delays are 30/120 seconds plus up to 10% jitter. C3 transport calls time out after five seconds with at most three attempts and 1/2-second delays. Transport retries do not invoke AI again. Failed posts retain raw input; expired leases reject late results. Source HTTP 429 honors Retry-After, otherwise waits 15 minutes.

### 3.5 Core Behavior and Shared Modules

**Eligibility and freshness:** Construct selected-day and previous-day occurrences; enforce distance <=1,609.344 meters and server-held age before serialization. For DST gaps use the first valid instant; for ambiguity use earlier start/later end. Signed cursors bind filters/time/order and expire after five minutes. Null checks or checks older than 24 hours are stale; old data remains labeled and visible. Retrying old text never refreshes its observation time.

**Publication:** Validate all owner fields before an atomic commit. Replay unchanged owner/key submissions; reject changed bodies or duplicate offers with 409. Automated confidence >=0.70 permits publication only with complete validated facts and approved enablement; 0.40-<0.70 is held, below 0.40 rejected. Treat uncertain alcohol as true. Duplicate matching requires equal schedule/category/alcohol/price/effective dates and title-token Jaccard >=0.80. Merge evidence without overwriting owner facts.

**G1/G2:** Use hashed 256-bit session tokens, secure HttpOnly SameSite=Lax cookies, CSRF checks, 24-hour guest expiry, and authenticated expiry <=30 minutes/provider expiry. Recheck owner mapping per request. Render escaped semantic HTML, keyboard controls, field errors, and accessible notices. Revalidate visible feeds every 60 seconds; hide alcohol after failed eligibility refresh. OQ-07 must resolve refresh/network delay against the 60-second publication target.

**G3/G4:** Validate required settings before readiness; keep unapproved web scrapers/AI disabled. Emit redacted structured events with a 1,000-event maximum local buffer and 14-day retention target. Drop oldest buffered events on collector failure and report the count after recovery. Target daily backups, seven-day retention, and one successful restore exercise. Monitor latency, failures, cost, and backlog older than 24 hours. These operational defaults remain unverified.

## 4. Technology Stack

| Layer | Choice | Requirement-based justification |
| --- | --- | --- |
| Web UI | Flask/Jinja, production WSGI server | Accessible feed/forms: REQ-01/05, NFR-04. |
| API | Python, FastAPI, Pydantic | Shared validation and typed errors: REQ-02/05. |
| Data | PostgreSQL, SQLAlchemy, Alembic | Transactions, uniqueness, migrations: REQ-06/07, CON-05. |
| Identity | Supabase Auth; application sessions | Verified owners and guest eligibility: NFR-03, CON-03. |
| Worker | APScheduler, database queue, httpx | Bounded recoverable work: NFR-05, CON-01. |
| AI / source | Anthropic SDK; web scrapers for approved Meta/X venue pages | Conditional scraping and extraction: REQ-07, CON-04. |
| Time | zoneinfo, python-dateutil | Explicit local schedule handling: REQ-01/09. |
| Hosting/tests | Azure containers/managed PostgreSQL; pytest, axe-core, browser tools | Recovery and verification: NFR-01/04/06/07. |

Exact versions, licenses, provider terms, model, and Azure SKU require confirmation before deployment.

## 5. Known Constraints

| ID | Design consequence |
| --- | --- |
| CON-01 | Require funded cap, model/quality approval, and atomic reservations. Pause AI at the cap; retain manual publication. |
| CON-02 | Owner publication works without any social account; social posting is not an onboarding requirement. |
| CON-03 | Prominent food access, server-enforced age confirmation, and no birth-date/ID-image collection. |
| CON-04 | Web scrape only approved venue pages; follow source access rules and request limits, stop on denied access, and do not bypass access controls. |
| CON-05 | Only C3 accesses C4 directly; clients and workers use its API. |

Open decisions cover web-scraping source approval and request limits (OQ-01), campus coordinates (OQ-02), price completeness (OQ-03), funding/AI evaluation (OQ-04), and benchmark/refresh conditions (OQ-07). Reconcile the Week 3 constraints and Week 8 plan under OQ-06.

Owners, proposed review dates, status, and resolution evidence for OQ-01-07 are maintained in [Requirements Document Section 9](Requirements_Document.md#9-assumptions-and-open-questions). OQ-05 records the existing deferred-feature decision.

D2 supersedes earlier client-supplied age, active-only default, and dropped-payload recovery. Bookmarks, moderation dashboards, and outbound notifications remain outside this release. These are inherited decisions, not new scope changes.

## 6. Traceability Matrix

Designed means specified, not implemented or tested. Planned identifies unconfirmed targets. Test IDs reserve Week 10 cases; no test results are claimed. Every baseline REQ/NFR/CON appears once with a named design element.

| Req ID | Description | Design Component (D0/D1/D2) | Status | Test Case |
| --- | --- | --- | --- | --- |
| REQ-01 | Day/radius/start-order feed | D2 B2 occurrence/radius query; C1 | Designed | TC-001 |
| REQ-02 | Guest age gate | D2 G1/WEB_SESSION; B2 age predicate | Designed | TC-002 |
| REQ-03 | Facts and provenance | D2 DealView/C1; DEAL_SOURCE | Designed | TC-003 |
| REQ-04 | Truthful stale labels | D2 B2 freshness metadata; G2 | Designed | TC-004 |
| REQ-05 | Owner publication | D2 C2/I2 validation; C4 transaction | Designed | TC-005 |
| REQ-06 | Safe submission retries | D2 REQUEST_RESULT; venue lock | Designed | TC-006 |
| REQ-07 | Conditional ingestion | D1 A; D2 A2/I3/I5; evidence | Designed | TC-007 |
| REQ-08 | Filters and paging | D2 C1/I1 filters; signed cursor | Designed | TC-008 |
| REQ-09 | Active-now view | D2 B2 interval/order logic | Designed | TC-009 |
| REQ-10 | Health reporting | D2 G3 settings; G4 health routes | Designed | TC-010 |
| NFR-01 | Feed under 3 seconds | D2 B2 indexes; C1 render; G4 timing | Designed | TC-011 |
| NFR-02 | Visibility within 60 seconds | D2 C2 commit/C1 refresh; OQ-07 | Designed | TC-012 |
| NFR-03 | Security controls | D2 G1/CSRF; HTTPS; DTO/log redaction | Designed | TC-013 |
| NFR-04 | Accessible pages | D2 C1 templates/keyboard; G2 notices | Designed | TC-014 |
| NFR-05 | Recoverable processing | D2 Figure 3 leases/attempts/replay | Designed | TC-015 |
| NFR-06 | Backups and restore | D2 C4 managed backup/restore policy | Planned | TC-016 |
| NFR-07 | Bounded logs | D2 G4 buffer/retention | Designed | TC-017 |
| NFR-08 | 100-times-volume stretch | D1 scale model; D2 B2 load-test plan | Planned | TC-018 |
| CON-01 | Funded AI enablement | D2 G3; BUDGET_RESERVATION | Designed | TC-019 |
| CON-02 | Owner-only venues | D2 C2/SUBMITTED_DEAL | Designed | TC-020 |
| CON-03 | Food and minimal age data | D2 C1 food filter; G1/B2 gate | Designed | TC-021 |
| CON-04 | Approved web-scraping sources | C6 source boundary; C5 scraper enablement; OQ-01 | Designed | TC-022 |
| CON-05 | C3-only database access | D2 I3/I4 credential separation | Designed | TC-023 |

All IDs map to a design. Open decisions still block acceptance; tests must include D2 EC-01-15 failure and boundary cases.
