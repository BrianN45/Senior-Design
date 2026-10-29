# BarCat Deals: Requirements Document

**Course:** CS 5001 - Senior Design I, University of Cincinnati  
**Team:** Brian Nguyen, Kaustubh Mathur, Ved Sanap, Meredith Bartel, Kiki Vasilev  
**Advisor:** Dr. Hrishikesh Vinayak Bhide  
**Version:** 0.2 | **Date:** October 28, 2026 | **Status:** Draft for review

## 1. Version and Change Log

| Version     | Date       | Change                                                                    | Author                 |
| ----------- | ---------- | ------------------------------------------------------------------------- | ---------------------- |
| 0.1         | 2026-10-28 | Consolidated stories and D0-D2 into requirements.                         | AI-assisted team draft |
| 0.1, rev. 1 | 2026-10-28 | Shortened prose/tables; preserved IDs, priorities, thresholds, and scope. | AI-assisted revision   |
| 0.2 | 2026-10-28 | Added assumptions and the OQ-01-07 register with owners and proposed review dates; preserved requirement IDs, priorities, and thresholds. | AI-assisted revision |

## 2. Purpose and Scope

BarCat Deals helps UC students compare food and bar offers near campus in one feed. Students browse today's deals, inspect details and sources, and receive age-appropriate results. Approved owners submit deals directly; web scraping of approved venue social-media pages supplements them when access, funding, and extraction quality are approved.

The release includes student feed/detail pages, an owner form, a central API/database, and conditional background ingestion. Today mode includes later-today offers, not only offers active now. Missing facts and stale data remain explicit.

Sources for this document:

- [D0 High-Level Design](Design_Diagrams/D0_Detailed_Design.md)
- [D1 Detailed Design](Design_Diagrams/D1_Detailed_Design.md)
- [D2 Detailed Design](Design_Diagrams/D2_Detailed_Design.md)

## 3. Stakeholders and User Goals

| Stakeholder | Goal | Source |
| --- | --- | --- |
| UC student, 21+ | Compare nearby food/alcohol deals in one place. | US-01; UC-01 |
| UC student, under 21 | Find affordable food without alcohol promotions. | US-02; AC-01.2 |
| Clifton venue owner | Submit deals without frequent social posting. | US-03; AC-03.1 |
| Student using assistive technology | Navigate and read the same deal details independently. | US-04; AC-04.1 |
| Team and advisor | Keep design feasible, funded, and traceable. | D2 1.2, 2.9, 5.1 |

## 4. Glossary

These definitions apply to both documents.

| Term | Meaning |
| --- | --- |
| UC; campus | University of Cincinnati; its approved main-campus reference point. |
| Student; guest | Intended student audience; visitor without a registered identity. Browsing needs no account. |
| Owner; maintainer | Approved representative mapped to one venue; authorized team operator. |
| Venue; deal/offer | Approved bar/restaurant; its scheduled promotion with classification, status, and source. |
| Published; pending review; rejected; expired | Publicly eligible; held/hidden; below acceptance criteria; past its last permitted occurrence. |
| Occurrence; effective dates | Concrete start/end interval; dates restricting permitted occurrence starts. |
| Today mode; active mode | Offers overlapping a selected local day; offers containing a selected instant. |
| Near campus | Straight-line distance <=1,609.344 meters from campus, not student location or walking distance. |
| Eastern time; UTC | America/New_York for schedules/display, including daylight saving time (DST); Coordinated Universal Time for stored instants. |
| Age confirmation; alcohol deal | Self-attested age 21+, not verified identity/age; alcohol offer, including uncertain automated classifications. |
| Last checked; stale | Validated observation or owner confirmation time; missing or older than 24 hours. |
| Provenance; source post | Owner/automated origin; original permitted post supporting extraction. |
| Ingestion; adapter; confidence | Background web scraping/extraction; site-specific normalization; model score from 0 to 1. |
| Session; CSRF | Server-held authorization context; protection against cross-site request forgery. |
| Idempotency key | Owner-scoped identifier that replays an unchanged submission's result. |
| Cursor; snapshot; lease | Pagination continuation; fixed calculation time, not database snapshot; temporary exclusive work permission. |
| MVP; MoSCoW | Minimum viable product; Must, Should, Could, Won't priorities. |
| REQ; NFR; CON; TC | Functional requirement; quality requirement; constraint; planned test case. |

## 5. Functional Requirements

Must is a release gate; Should requires a logged decision to defer; Could is optional. Selected features still obey Must requirements and constraints. Test IDs reserve Week 10 cases, not completed results.

| ID | Requirement | MoSCoW | Source story / basis | Verification method |
| --- | --- | --- | --- | --- |
| REQ-01 | The system shall expose every published, age-eligible offer overlapping the selected Eastern day within 1,609.344 meters, ordered by occurrence start then ID, including later-today and overnight offers. | Must | US-01/02; AC-01.1/2; D2 2.5 | TC-001: compare all paginated IDs; test radius, midnight, effective dates, and DST. |
| REQ-02 | The system shall allow account-free age confirmation, default absent/declined/dismissed answers to false, and expose zero alcohol content in feeds/details without valid positive server-held confirmation; explicit unauthorized alcohol requests return 403. | Must | US-02; UC-01 A1; AC-01.2 | TC-002: guest, false, forged, expired, and revoked sessions through UI/API. |
| REQ-03 | The system shall show venue, offer, price text, occurrence times, and last check for every deal, with details linking the original HTTPS post or stating “Submitted by venue”; absent prices read “Price not provided.” | Must | US-01/04; UC-01; AC-01.1 | TC-003: both sources and missing fields; price completeness remains OQ-03. |
| REQ-04 | The system shall show the matching dataset's last update above the feed, label checks older than 24 hours “Possibly out of date” and null checks “Never verified,” without hiding otherwise eligible stale offers. | Must | US-01; AC-01.3 | TC-004: 24 hours exactly/+1 second, null/mixed ages, empty feed; metadata spans all pages. |
| REQ-05 | The system shall publish approved-owner deals using seven required items, reject missing/invalid values with named text errors and zero committed deals, derive venue authority from the owner mapping, and confirm only after commit. | Must | US-03; AC-03.1 | TC-005: omit each item; test profiles, cross-venue attempts, and rollback. |
| REQ-06 | The system shall replay the original response for unchanged owner/key submissions for 24 hours with at most one deal; changed bodies or duplicate offers under new keys return 409. | Should | US-03; D2 2.3-2.4 | TC-006: double-click, lost response, retry, concurrent duplicates. |
| REQ-07 | The system shall, when approved web scraping and AI extraction are enabled, register posts once per platform/external ID, extract 0-10 deals/post, publish only complete schema-valid results with confidence >=0.70, and retain hidden uncertain results with provenance. | Should | US-01; UC-01; D1 A/D2 A2 | TC-007: valid, empty, malformed, duplicate, and uncertain inputs; OQ-01/04 gate web scraping and AI enablement. |
| REQ-08 | The system shall filter by venue or food/drink/other category, provide a prominent food filter, and paginate 1-50 results/page without widening the radius or bypassing age eligibility. | Should | US-01/02; D2 2.6 | TC-008: filtered sets, invalid limits, altered, expired, or mismatched cursors. |
| REQ-09 | The system shall offer active mode with start <= requested instant < end, ordered by food first, remaining minutes, then ID. | Could | US-01; D1 B/D2 B2 | TC-009: interval boundaries, overnight offers, minute rounding, order. |
| REQ-10 | The system shall return liveness 200 and readiness 200 only with valid mandatory settings/database access, otherwise readiness 503, exposing zero secrets. | Should | US-01/03; D2 2.9 | TC-010: missing configuration/database outage; inspect redaction. |

The seven owner items are business name, address, description, category, days/times, alcohol yes/no, and contact email. Days/times expands into weekdays and unequal start/end times. Title and price remain optional under D2, leaving OQ-03 unresolved.

## 6. Non-Functional Requirements

NFR-01 sampling/concurrency and NFR-08 are proposed validation conditions. D2 operational defaults require confirmation; inherited acceptance thresholds remain unchanged.

| ID | Requirement | MoSCoW | Source story / basis | Verification method |
| --- | --- | --- | --- | --- |
| NFR-01 | The system shall render the initial feed in <3 seconds for at least 95/100 visits with 50 venues, about 300 published deals, and 5 concurrent sessions. | Must | US-01; AC-01.1; D2 2.5 | TC-011: navigation-to-card timing; approve network profile/sampling in OQ-07. |
| NFR-02 | The system shall show valid currently eligible owner submissions within 60 seconds in every acceptance fixture, including already-open visible feeds. | Must | US-03; AC-03.1 | TC-012: submit-to-visible timing on new/open feeds, including refresh/network delay. |
| NFR-03 | The system shall reject 100% of tested unauthorized protected access and CSRF-invalid browser mutations, use HTTPS on HTTP boundaries, and expose zero tokens, credentials, or owner emails in public deal responses/logs. | Must | US-02/03; D2 G1/G4 | TC-013: authority, cross-venue, CSRF, transport, response/log checks. |
| NFR-04 | The system shall have zero serious/critical axe-core findings on feed/details under WCAG 2.1 AA checks and support 100% of defined browse/detail tasks by keyboard with zero traps. | Must | US-04; AC-04.1 | TC-014: scan loaded, empty, restricted, stale, and error states; keyboard/screen-reader walkthrough. |
| NFR-05 | The system shall retain failed raw posts, permit <=3 processing attempts with <=1 AI call each, reject all expired/replaced lease completions, and commit at most one result/post. | Should | US-01; D2 2.7 | TC-015: worker crashes, lease expiry, replay, AI timeout. |
| NFR-06 | The system shall back up at least daily, retain backups >=7 days, and pass one isolated pre-release restore with all foreign-key/provenance checks passing. | Should | US-01/03; D2 2.4 | TC-016: inspect policy and restored data; hosting capability remains unverified. |
| NFR-07 | The system shall cap its local log buffer at 1,000 events and retention at 14 days, dropping oldest entries during collector failure and reporting the dropped count after recovery. | Should | US-01/03; D2 2.9 | TC-017: collector outage, >1,000 events, continued browsing, retention/drop checks. |
| NFR-08 | The system shall meet NFR-01 timing/concurrency with 5,000 venues, 30,000 published deals, and about 5 million historical rows as an optional stretch goal. | Could | US-01; D1 scale/D2 test plan | TC-018: repeat at 100-times volume; failure does not fail MVP. OQ-07 approval pending. |

## 7. MoSCoW Summary

There are 18 requirements: 9 Must, 7 Should, and 2 Could. The core student/owner flows and acceptance gates take priority; deferring automation or operational extras cannot relax constraints.

- **Must:** REQ-01-05; NFR-01-04.
- **Should:** REQ-06, REQ-07, REQ-08, REQ-10; NFR-05-07.
- **Could:** REQ-09; NFR-08.
- **Won't this release:** bookmarks/saved-deal routes; moderation dashboard; email/SMS/push; self-service venue claiming; mandatory student registration; birth-date/ID-image collection; walking-distance routing; bypassing source access controls or request limits.

## 8. Inherited Constraints

CON-01-03 reflect D2's economic/social/ethical constraints; CON-04-05 are inherited design constraints. Reconcile the original Week 3 essay under OQ-06.

| ID | Constraint | Evidence / verification |
| --- | --- | --- |
| CON-01 | Stay within funded hosting/identity/AI limits. Keep AI publication off until a monthly cap, model, and quality gate are approved; pause before exceeding reserved budget. No dollar cap is established. | D1 6.5; D2 G3. TC-019: cap denial and manual fallback; OQ-04. |
| CON-02 | Approved owners can publish without social accounts or frequent posts. | D2 CON-SOCIAL. TC-020: owner-only venue during provider outage. |
| CON-03 | Keep food prominent and age gating server-side; collect no birth dates/ID images. | D2 CON-ETHICAL/G1. TC-021: UI, authorization, and stored-field inspection. |
| CON-04 | Web scrape only approved venue pages; follow source access rules and request limits, stop on denied access, and do not bypass access controls. | OQ-01; D2 DR-08 access restrictions. TC-022: source approval, request limits, and disabled scrapers. |
| CON-05 | Only C3 directly accesses C4; clients/C5 use its interfaces and migrations follow C3's controlled path. Shared modules stay within existing components. | D2 I3/I4. TC-023: credentials, network policy, calls. |

## 9. Assumptions and Open Questions

These entries consolidate the existing D2 design and unresolved acceptance conditions. Assumptions are planning premises, not verified facts. Owners below coordinate confirmation; new follow-up allocations and the October 30 review dates are proposed for team confirmation, not agreed deadlines or claims of completion. Review may leave an item open with a recorded next action.

### 9.1 Assumptions

| ID | Assumption and validation dependency | Owner | Date recorded |
| --- | --- | --- | --- |
| ASM-01 | The approved UC main-campus reference point and venue coordinates support a fixed one-mile straight-line boundary; student GPS is unnecessary. Coordinates remain subject to OQ-02. | Meredith Bartel; Brian Nguyen confirms the campus point | 2026-10-28 |
| ASM-02 | The team can onboard approved owners and maintain their venue mappings. Owner submissions remain usable without a social-media account. Validate with REQ-05 and CON-02 fixtures. | Kaustubh Mathur | 2026-10-28 |
| ASM-03 | Guest age confirmation is self-attestation without registration, birth dates, or ID images. It does not establish verified age. Validate REQ-02 and CON-03 through UI/API checks. | Ved Sanap | 2026-10-28 |
| ASM-04 | Approved web-scraping access, funded service limits, and acceptable extraction quality can be demonstrated before automation is enabled. Until OQ-01/04 close, web scrapers and AI publication remain disabled as applicable; manual publication remains available. | Kiki Vasilev | 2026-10-28 |
| ASM-05 | About 50 venues and 300 published deals represent the initial performance fixture. Sampling, concurrency, network conditions, and the optional 100-times workload remain proposed under OQ-07. | Ved Sanap | 2026-10-28 |

### 9.2 Open Questions and Deferred Decisions

OQ-01 now addresses web scraping of venue social-media pages; D2's API-based source collection is historical. OQ-02-05 retain their D2 meanings. OQ-06-07 define the follow-ups already cited in this document and the technical specification.

The current rules below are proposed working decisions for this draft. They do not claim that access, coordinates, funding, or test results have been verified. An open item closes only when its owner records the required evidence or agreement and reconciles any affected acceptance criteria or technical specification. Owners and proposed review dates remain unchanged.

| ID | Question and resolution evidence | Coordinating owner | Proposed review date | Status / current rule |
| --- | --- | --- | --- | --- |
| OQ-01 | Which venue social-media pages can be web scraped, and what access rules, request limits, and costs apply? Test one approved venue page before enabling its scraper. | Kiki Vasilev | 2026-10-30 | Open; web scrape approved venue pages after owner onboarding and source approval. Follow source request limits and stop if access is denied or the funded limit is reached. Keep manual submissions available when scraping is paused. |
| OQ-02 | What campus reference point and venue coordinates are approved? Seed and verify venue coordinates and record Brian's confirmation of the campus point before geographic acceptance testing. | Meredith Bartel, with Brian Nguyen | 2026-10-30 | Open; propose the MainStreet entrance of [Tangeman University Center](https://www.uc.edu/campus-life/tuc.html) as the campus point, with Brian's approval. Store the verified point in version-controlled configuration and verify venue coordinates during onboarding. Missing campus coordinates block readiness; unverified venues stay unpublished. |
| OQ-03 | Can a published deal omit a price while satisfying AC-01.1? Agree the price policy with stakeholders and reconcile REQ-03 with that criterion. | Kaustubh Mathur | 2026-10-30 | Open; keep owner prices optional. Show the supplied price or "Price not provided"; never guess. Hold automated deals with missing prices. |
| OQ-04 | What funded hosting/identity/AI limits, approved model, and extraction-quality pass criteria permit automation? Record service/operating capabilities, budget caps, labeled-post evaluation results, and approval to enable publication. | Ved Sanap, with Kiki Vasilev for AI/access evidence | 2026-10-30 | Open; use at most 80% of confirmed service funding and keep 20% in reserve. Make no unfunded paid calls. Reserve AI-call costs before spending and stop at the cap. Before enabling AI, test 100 unseen posts: >=95% of published deals must be correct, with no wrong alcohol, time, or price details. Publishing nothing cannot pass. Record the model, results, service checks, and team approval. |
| OQ-05 | Should bookmarks or a moderation dashboard enter a later release? Reconsider only against an accepted story, acceptance criteria, and a logged scope decision. | Team; Brian Nguyen coordinates | 2026-10-30 | Deferred; leave bookmarks and a moderation dashboard out of the MVP. Maintainers correct and reprocess held or failed extractions. Add future features only with a stakeholder story, testable criteria, an owner, and recorded team approval. |
| OQ-06 | Do CON-01-05 and the Week 8 task ownership/schedule match the inherited constraints and required work? Reconcile [Constraints](Constraints.md), the [Task List](Planning/Tasklist.md), and the [Planning PDF](Planning/Planning.pdf); log any discrepancy and agreed resolution. | Ved Sanap, with component owners | 2026-10-30 | Open; follow CON-01-05 and the D0/D2 component owners. Each task needs a component, REQ/NFR/CON reference, one owner, dependencies, and a one-to-two-week work period. Resolve plan conflicts before work starts and record owner or scope changes. Verify funding rather than assuming services are free. |
| OQ-07 | What performance sample, concurrency, network profile, and stretch-test policy are accepted? Confirm NFR-01/08 conditions and an end-to-end refresh budget that can meet NFR-02's 60-second limit including processing, transfer, and rendering. Log any acceptance change. | Ved Sanap, with Brian Nguyen for browser timing | 2026-10-30 | Open; test 100 initial feed loads with five concurrent users, 50 venues, and 300 deals at 10 Mbps download, 1 Mbps upload, and 100 ms round-trip delay. Require every load under three seconds. Refresh visible feeds every 30 seconds, allowing at most 30 more for commit, fetching, and rendering; verify each eligible submission appears within 60 seconds. Keep 100-times testing optional. Update the technical specification's 60-second refresh interval before closing this item. |
