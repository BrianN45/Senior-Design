# BarCat Deals: Test and Validation Plan Draft

**Course:** CS 5001 - Senior Design I, University of Cincinnati  
**Team:** Brian Nguyen, Kaustubh Mathur, Ved Sanap, Meredith Bartel, Kiki Vasilev  
**Advisor:** Dr. Hrishikesh Vinayak Bhide  
**Version:** 0.1 | **Date:** November 4, 2026 | **Status:** AI-assisted draft; team review pending  
**QA/test lead:** Kaustubh Mathur, following Week 8 task 02 and milestone F08; component owners review their cases.

## 1. What is being tested this cycle

This draft plans CS 5002 execution against [Requirements v0.2](Requirements_Document.md) and [Technical Specification v0.4](Technical_Specification.md), supported by [User Stories](User_Stories.md) and [D0](Design_Diagrams/D0_Detailed_Design.md)/[D1](Design_Diagrams/D1_Detailed_Design.md)/[D2 v1.2](Design_Diagrams/D2_Detailed_Design.md). Current contracts supersede historical API-only collection, bookmarks, and active-only defaults. No implementation or passing results are claimed.

| In scope | Requirement IDs | Component / primary owner |
| --- | --- | --- |
| Today feed, facts, freshness, filters, accessible feedback | REQ-01, 03, 04, 08; NFR-01, 04 | C1/G2: Brian Nguyen; C3: Ved Sanap |
| Approved-owner publication, validation, safe retry, visibility | REQ-05, 06; NFR-02 | C2: Kaustubh Mathur; C3/C4: Ved/Meredith |
| Guest age, owner authority, service boundaries, health | REQ-02, 10; NFR-03; CON-03, 05 | C3/G1: Ved Sanap; C4/G4: Meredith Bartel |
| Conditional approved-page scraping, extraction, recovery, spending | REQ-07; NFR-05; CON-01, 02, 04 | C5/C6 boundary/G3: Kiki Vasilev |
| Database backup/restore and bounded logging | NFR-06, 07 | C4/G4: Meredith Bartel |

**Major requirements:** all nine Must requirements (REQ-01-05, NFR-01-04), plus REQ-06/07 and NFR-05 because duplication and unsafe automation threaten data integrity. Each has three cases, including a boundary or failure. All other in-scope IDs have at least one case.

**Out of scope:** REQ-09 active mode and NFR-08 100-times load are deferred Could features; TC-009/018 remain reserved. Won't features: bookmarks, moderation UI, email/SMS/push, self-service venue claiming, mandatory student accounts, DOB/ID images, walking routes, and access-control bypasses. Test our Supabase/Azure/Anthropic/source integrations, not provider internals or global availability. There are no sensors. Limits: self-attestation cannot verify age; radius is straight-line; fixtures cannot prove real-world coverage. Live scraping/AI requires OQ-01/04 approval.

## 2. Verification and validation strategy

**Verification:** component owners use pytest, real PostgreSQL integration tests, browser automation and axe-core. Test age/authority and atomic writes first, then geography/time, faults, performance and presentation. Unit tests isolate dependencies; integration tests exercise real constraints, concurrency and restores. Passing stubs cannot establish live-service acceptance.

**Validation:** recruit two students (one choosing the under-21 flow), an approved-owner representative and a keyboard/screen-reader user. Students locate and explain price/time/source/freshness (TC-003A/B, 004C); the owner submits and observes a deal (TC-005A, 012A); the assistive-technology user runs TC-014C. Target unaided completion of every listed task. Record completion, misunderstandings and accept/revise decisions against US-01-04; ask the advisor to review results/policies. This proposed small-sample protocol adds no approved NFR. Recruitment and sign-off are pending.

| Layer | Approximate checks after parameterization | Coverage / execution point |
| --- | --- | --- |
| Unit | 80 | Validation, occurrences, distance, age, matching, redaction; each commit |
| Integration | 30 | I1-I5, real C4 transactions, sessions, leases, budgets; each pull request |
| System | 12 | Integrated browser flows, faults, accessibility, timing, restore; milestone candidate |
| Acceptance | 4 walkthroughs | Student, nonalcohol, owner, assistive-technology needs; before advisor/demo sign-off |

Counts are estimates; cases expand into checks and may span layers. Section 5 fixes AI variables, metrics and trials; confidence/schema validity alone cannot establish factual accuracy.

<!-- pdf-page-break -->

## 3. Measurable success criteria

Compare unrounded values; a boundary equal to a strict upper limit fails. Each metric is tied to the Week 9 baseline. Proposed operating settings require recorded approval before acceptance.

| Metric | Target threshold | Measurement / requirement |
| --- | --- | --- |
| Eligible feed completeness and order | 100% expected IDs, no extras/duplicates; distance <=1,609.344 m; start then ID order | Independent occurrence/distance oracle across all pages; REQ-01; TC-001A-C |
| Age and protected access | 0 unauthorized alcohol disclosures; 100% tested unauthorized/CSRF-invalid actions denied; 0 secret/email leaks | Response bodies, HTML, browser state, logs, transport inspection; REQ-02; NFR-03; TC-002A-C, 013A-C |
| Facts, sources, freshness | 100% cards/details have required labels; exact provenance; stale only >24 h, null = Never verified; no stale-only removal | Fixture assertions, all-page summary oracle; REQ-03/04; TC-003A-C, 004A-C; price acceptance gated by OQ-03 |
| Owner validation and atomicity | 7/7 omission trials name the item and commit 0 deals; valid input commits exactly 1 before success | UI/API responses and C4 before/after counts; REQ-05; TC-005A-C |
| Retry safety | <=1 deal across retries/concurrent copies; unchanged key replays original response for 24 h; altered body/new-key duplicate =409 | Response comparison and row counts; REQ-06; TC-006A-C |
| Automated result integrity | One post/platform/external ID; 0-10 deals/post; complete validated facts and confidence >=0.70 to publish; .40-<.70 held, <.40 rejected | Fixed model stubs and database/provenance inspection; REQ-07; TC-007A-C |
| AI factual accuracy (proposed) | >=95% correct published deals; 0 wrong published alcohol, time, or price facts; zero publication cannot pass | 100 unseen labeled posts, exact factual comparison, report publication coverage and omissions; REQ-07, CON-01; OQ-04; TC-007C, 019 |
| Feed performance | >=95/100 initial loads <3 s with 5 concurrent sessions, 50 venues / 300 published deals | Browser navigation-to-complete-first-page-card-render, 20 batches of 5 cold visits; NFR-01; TC-011A-C. Also report 100/100 proposal under OQ-07 separately |
| Publication visibility | Every eligible fixture <=60 s from submit to visible card on new and already-open visible feeds | One browser monotonic clock for submit + observation; include processing/network/render; NFR-02; TC-012A-C |
| Accessibility/usability | 0 critical/serious axe-core findings per tested page/state; 100% defined keyboard tasks, 0 traps | WCAG 2.1 A/AA axe rules, saved report and manual checklist; NFR-04; TC-014A-C |
| Recovery and operations | <=3 processing attempts, <=1 AI call/attempt; 0 accepted stale completions; <=1 committed result/post; daily backups, >=7-day retention, 1 successful isolated restore | Fault traces, AI-call counters, row counts, backup policy and restored integrity; NFR-05/06; TC-015A-C, 016 |
| Health, logs, controls | Live=200; ready=200 only with mandatory settings/DB, else 503; buffer <=1,000, retention <=14 days, exact drop count; 0 unfunded/unapproved calls | Configuration/fault injection, clock advance, network/DB access probes; REQ-10; NFR-07; CON-01-05; TC-010, 017, 019-023 |
| Filters and pagination | Exact expected sets; limit 1-50, default 20; 0 eligibility bypass; invalid/altered/expired/mismatched cursor=422 | Compare all pages at limits 1/20/50; signed cursor expires at five minutes; REQ-08; TC-008 |

## 4. Test cases

Use the five columns below. TC-001A means case A in existing Week 9 group TC-001; no group is reassigned. All 47 cases are **Not Run** because implementation/execution is future work. On execution use only Pass, Fail, Blocked, or Not Run, with evidence under Section 6. Bracketed U/I/S/A identify unit, integration, system, or acceptance use. Fixture names and shared reset/capture steps are defined in Section 5; numbered actions are sequential. Requirements and D2 edge-case IDs appear in descriptions.

<!-- pdf-page-break -->

### 4.1 Core browsing and owner publication

| ID | Description | Steps | Expected Result | Status |
| --- | --- | --- | --- | --- |
| TC-001A | REQ-01 today set/order [I/S] | 1. Reset F at T0; confirm age=true. 2. GET today for 2026-11-04 at limit=1 until cursor=null. 3. Compare API IDs and UI next pages. | Exactly C,A,G,B,F in that order; later-today B/F and previous-day overnight C included; D/E absent; no repeats. | Not Run |
| TC-001B | REQ-01 radius/effective-day edges; EC-05, EC-06 [U/I] | 1. Evaluate R at the four recorded distances. 2. Query C on Nov 4 with final valid start date Nov 3. 3. Move its final date to Nov 2; repeat. | First three distances included, 1,609.345 m excluded; C included before change and excluded after. End on final occurrence, not midnight of its start day. | Not Run |
| TC-001C | REQ-01 DST boundaries; EC-07 [U/I] | 1. Reset DST fixture clock to each test date. 2. Construct Mar 14 02:30-04:00 and Nov 7 01:30-01:45 occurrences in 2027. 3. Compare stored/returned UTC intervals. | Spring interval 07:00-08:00Z; fall interval 05:30-06:45Z; each returned once in selected-day feed. No nonexistent local instant. | Not Run |
| TC-002A | REQ-02 absent/declined/dismissed age; EC-01 [I/S/A] | 1. Reset F and use fresh browser for each of no session, declined, dismissed. 2. Browse feed/all pages and alcohol detail F. 3. Explicitly request include_alcohol=true. | Feed contains C,A,G,B, 0 alcohol-offer content/IDs; food retained; detail and explicit alcohol request=403; browsing needs no registration. | Not Run |
| TC-002B | REQ-02 server authority [I/S] | 1. With age=false, forge age/role in query, JSON and browser storage. 2. Request alcohol feed/detail. 3. Confirm via valid CSRF-protected age endpoint and repeat. | Forgery discloses 0 alcohol and explicit requests=403; valid server confirmation permits F; no DOB/ID requested. | Not Run |
| TC-002C | REQ-02 expiry/revocation; EC-01/15 [I/S] | 1. Display F with confirmed session. 2. Repeat independently with expired and revoked token. 3. Trigger visible-feed revalidation, then request detail directly. | Invalid supplied token=401; revalidation removes alcohol cards and offers session renewal; 0 alcohol in failed response or refreshed DOM. | Not Run |
| TC-003A | REQ-03 facts/source [S/A] | 1. Reset F. 2. Open owner A and automated B cards/details. 3. Read venue, offer, price, occurrence and check time; follow B's local HTTPS source stub. | All five fact labels match F; A says Submitted by venue; B links exactly its source and matching post, with no invented fact. | Not Run |
| TC-003B | REQ-03 missing price; OQ-03 [S/A] | 1. Remove A's optional price. 2. Open its card/detail. 3. Submit extraction stub missing price for B. | Owner shows Price not provided; no guessed value; automated missing-price candidate hidden. Technical check can pass; full price-policy acceptance remains blocked pending OQ-03. | Not Run |
| TC-003C | REQ-03 unsafe content [I/S] | 1. Put literal script/HTML in A's description. 2. Send automated source URLs with http: and javascript: schemes. 3. Open rendered detail. | Description rendered as text, 0 script executions; unsafe sources rejected/held with 0 public unsafe links; valid HTTPS provenance preserved. | Not Run |
| TC-004A | REQ-04 24-hour boundary; EC-05 [U/S] | 1. Set A/B/C checks to T0-24 h, T0-24 h-1 s, and null. 2. Browse at T0. 3. Read labels and returned IDs. | A fresh; B Possibly out of date; C Never verified; all otherwise eligible IDs remain present. | Not Run |
| TC-004B | REQ-04 all-page metadata [I/S] | 1. Give A/G old checks and B the newest check T0. 2. Query F limit=1 through all pages. 3. Compare summary with independent full-set oracle. | Every page reports same full-set last update T0 and stale count, even when newest B is on a later page; no per-page summary substitution. | Not Run |
| TC-004C | REQ-04 empty/unverified; EC-08 [I/S/A] | 1. Query empty fixture. 2. Set every eligible check to null and repeat. 3. Explain each above-feed notice to observer. | Empty success: data=[], cursor/update=null, stale_count=0. All-unverified: update=null, count equals matching deals; notice states never verified; no false fresh/successful-update claim. | Not Run |
| TC-005A | REQ-05 seven-item success [I/S/A] | 1. Reset O; sign in approved owner OA. 2. Submit valid seven-item form with key K. 3. Read committed deal/result rows and UI. | 201 only after commit; exactly one deal, one owner subtype and request result; approved mapped venue VA; success notice identifies committed deal. | Not Run |
| TC-005B | REQ-05 missing/invalid fields; EC-02/07 [U/I/S] | 1. Reset before each of seven logical-item omissions. 2. Repeat with description length 1,001, weekday 0 and equal times. 3. Submit UI and direct API. | Every invalid trial=422 at API with named text field error and 0 new deals; UI preserves other inputs. False alcohol is a valid value, not an omission. | Not Run |
| TC-005C | REQ-05 authority/rollback; EC-04/08 [I/S] | 1. As OA send forged venue VB, then mismatched profile. 2. As nonowner attempt submit. 3. Inject DB failure before commit of valid form. | Forged/mismatched input=422, role=403; DB failure=503; 0 committed deals/subtypes/request results in each trial; no UI success, entered form retained. | Not Run |

<!-- pdf-page-break -->

### 4.2 Retry safety and Must quality requirements

| ID | Description | Steps | Expected Result | Status |
| --- | --- | --- | --- | --- |
| TC-006A | REQ-06 unchanged-key replay; EC-03/09 [I] | 1. Submit O with K. 2. Drop response; retry identical body/K at +1 s and +23h59m59 s using valid owner session. 3. Compare responses/rows. | Original status/body/ID replayed; exactly 1 deal and original result; no second publication. | Not Run |
| TC-006B | REQ-06 conflicts [I] | 1. Commit K. 2. Change description with same K. 3. Send original duplicate under new key. | Both=409; exactly 1 deal; original result unchanged; error distinguishes conflict without overwriting facts. | Not Run |
| TC-006C | REQ-06 concurrent duplicate; EC-10 [I] | 1. Release 10 identical OA submissions simultaneously, same K. 2. Repeat with 10 distinct keys for same offer. 3. Count rows. | One deal per trial; same-K responses replay same result; new-key losers=409; 0 partial subtypes. | Not Run |
| TC-011A | NFR-01 representative latency [S] | 1. Reset P; apply Section 5 network/reference staging. 2. Run 20 batches of 5 concurrent cold feed visits, closing contexts each batch. 3. Save 100 complete-render durations and errors. | At least 95/100 <3,000 ms; all successful pages show full first-page cards. Errors/incomplete renders count as slow failures; report max and all samples, not API-only time. | Not Run |
| TC-011B | NFR-01 denied dependencies; EC-08 [S] | 1. Repeat latency trial setup. 2. Stop API, then separately DB, and attempt 5 visits each. 3. Restore and rerun TC-011A. | Each failed visit has explicit error/retry and no empty-success claim; no failed sample discarded. Acceptance needs a new complete passing 100-visit run after repair. | Not Run |
| TC-011C | NFR-01 exact threshold [U/S] | 1. Feed timing evaluator 94, 95 and 100 samples <3,000 ms out of 100; all remaining =3,000 ms. 2. Apply evaluator to raw TC-011A run; report stricter OQ-07 count. | 94 fails; 95/100 pass baseline; 3,000 ms is not under 3 s. 100/100 is reported separately; no silent replacement of either acceptance rule. | Not Run |
| TC-012A | NFR-02 new-feed visibility [S/A] | 1. Reset O with currently eligible nonalcohol deal. 2. Start browser clock at Submit; post form. 3. From second page in same browser, poll new feed navigations every second until card fully renders. | Committed ID visible <=60,000 ms after submit in each of 10 trials; no precommit visibility or success. Polling cost is included. | Not Run |
| TC-012B | NFR-02 already-open worst phase [S] | 1. Keep student feed visible. 2. Submit just after a scheduled refresh starts. 3. Repeat 10 times with configured refresh and shaped network; time to automatic card render. | Each of 10 <=60,000 ms without manual reload. Record actual interval; a 60-second refresh plus processing cannot be excused by configuration. | Not Run |
| TC-012C | NFR-02 failure/retry; EC-03/08 [I/S] | 1. Drop owner response after commit. 2. Retry same key/body. 3. Observe open and new feeds as in A/B, starting time at original Submit. | Original committed ID replayed; exactly one deal; visibility <=60,000 ms from original submit in 10/10 trials; retry does not restart timer. | Not Run |
| TC-013A | NFR-03 unauthorized matrix; EC-04, EC-15 [I] | 1. For each protected owner/worker route send absent, forged and wrong-role credentials. 2. Repeat session routes with expired/revoked sessions. 3. Query DB changes. | All tested unauthorized actions denied (401 invalid authentication; 403 insufficient role/authority); 0 unauthorized mutations/disclosures; cross-venue body/profile validation separately covered by TC-005C. | Not Run |
| TC-013B | NFR-03 CSRF/transport [I/S] | 1. Send every browser mutation with missing/wrong CSRF and cross-site origin. 2. Inspect cookies and HTTPS boundaries. 3. Probe plain HTTP. | 100% invalid-CSRF mutations=403 before write; Secure/HttpOnly/SameSite=Lax cookies; plain HTTP redirects to HTTPS or is refused before sensitive content; no credentials sent over HTTP. | Not Run |
| TC-013C | NFR-03 privacy/redaction [I/S] | 1. Seed unique canary token/key/email. 2. Exercise success, validation, login and DB/AI errors. 3. Search public JSON/HTML/logs and inspect no-store headers. | 0 canary secrets/emails in public deals/logs; 0 raw posts/images in logs; Cache-Control: no-store on deal/session responses; errors retain request ID without secrets. | Not Run |
| TC-014A | NFR-04 loaded pages [S] | 1. Load F feed and A/B details in reference browser. 2. Run pinned axe-core WCAG 2.1 A/AA rules after render. 3. Save per-page reports. | Every page has 0 critical and 0 serious violations; unresolved incomplete checks receive manual disposition; scan failure is not a clean result. | Not Run |
| TC-014B | NFR-04 adverse states; EC-01/08 [S] | 1. Render empty, restricted, stale and API-error feed/detail states. 2. Run same axe configuration on each. 3. Inspect notices/focus. | Each state has 0 critical/serious findings; notices readable without color alone; no hidden alcohol exposed in restricted state. | Not Run |
| TC-014C | NFR-04 keyboard/screen reader [S/A] | 1. Unplug mouse; use Tab/Shift-Tab/Enter/Space to dismiss age, select food, follow Next, open detail, return. 2. Repeat with screen reader reading facts/source/stale notice. 3. Record each task. | 5/5 navigation tasks complete, 0 traps; visible focus and meaningful names; venue/offer/price/time/check/source announced correctly. No observer intervention for task completion. | Not Run |

<!-- pdf-page-break -->

### 4.3 Supporting requirements, recovery, and constraints

| ID | Description | Steps | Expected Result | Status |
| --- | --- | --- | --- | --- |
| TC-007A | REQ-07 normalization/limits [U/I] | 1. Register same normalized post twice and with another platform. 2. Complete posts with 0, 10 and 11 candidates; test text 5,001 chars/11 images. | Same platform/ID registered once; other platform distinct; 0 means no_deal, 10 valid allowed; oversized inputs=422/held, 0 partial invalid-batch publications. | Not Run |
| TC-007B | REQ-07 confidence/validation; EC-12 [U/I] | 1. With approved test settings, complete valid stubs at .3999/.40/.6999/.70. 2. Repeat .99 with invented/invalid date, missing price and ambiguous alcohol. | Thresholds reject/hold/hold/publish respectively; malformed/missing facts never published; uncertain alcohol=true; raw post and provenance retained. A high score cannot bypass validation. | Not Run |
| TC-007C | REQ-07 factual evaluation [I/A] | 1. Freeze H labels/model/prompt/settings. 2. Run each of 100 posts once; two reviewers compare published candidates to evidence. 3. Calculate Section 3 accuracy and coverage; keep publisher off until approved. | >=95% fully correct published deals and 0 wrong alcohol/time/price; 0 published fails; report held/rejected/missed counts. OQ-04 approval needed; no score-based accuracy claim. | Not Run |
| TC-008 | REQ-08 filters/cursors; EC-07 [I/S] | 1. Query F by venue and each category at limits 1/20/50. 2. Try 0/51 and altered, five-minute-expired, changed-filter cursors. 3. Repeat age=false. | Exact oracle sets; food filter prominent; default20; invalids=422; no duplicates, widened radius, or unauthorized alcohol. | Not Run |
| TC-010 | REQ-10 health; EC-08 [I] | 1. Probe live/ready normally. 2. Remove mandatory campus setting, then restore and deny DB separately. 3. Inspect bodies. | Normal both200; each fault ready503/live200 while process running; no secrets; recovery restores ready200. | Not Run |
| TC-015A | NFR-05 crash/lease loss; EC-11 [I] | 1. Claim post, kill worker before completion. 2. Advance past 120 s lease; reclaim. 3. Submit old token, then current result. | Old completion=409; only current lease can commit; exactly1 stored result; raw post survives crash. | Not Run |
| TC-015B | NFR-05 retry limits; EC-13 [I] | 1. Stub AI timeout on every call. 2. Run scheduler through all retries with controlled clock. 3. Count calls/attempts; inspect failed post. | <=3 attempts, <=1 AI call each; 30/120 s delays plus 0-10% jitter; after third failure raw retained, no fourth call/publication. | Not Run |
| TC-015C | NFR-05 completion replay; EC-09/10 [I] | 1. Commit valid completion then lose response. 2. Retry same completion; race two workers on matching offers. 3. Inject deadlock twice. | Replay identical result; <=1 published offer with preserved evidence; one deadlock retry then503, no partial commit; late/replaced tokens409. | Not Run |
| TC-016 | NFR-06 isolated restore [I/S] | 1. Inspect >=8 days of scheduled backup evidence. 2. Restore latest backup into isolated DB. 3. Compare snapshot counts/checksums, foreign keys and provenance; run migration-failure rollback trial. | Backup gap <=24 h, retention >=7 days; one restore succeeds with 0 FK/provenance/checksum differences; failed migration leaves prior version usable; no production overwrite. | Not Run |
| TC-017 | NFR-07 collector outage [I] | 1. Deny collector; emit numbered events 1-1,205; browse. 2. Restore collector. 3. Advance retention clock beyond14 days. | Buffer never>1,000; retained 206-1,205; drop count205 reported; feed remains usable; no events older than14 days retained. | Not Run |
| TC-019 | CON-01 funding gate; EC-13 [I/A] | 1. Disable funding approval/model, then test cap with concurrent reservations crossing remaining balance. 2. Deny AI budget. 3. Submit owner deal. | 0 unapproved/unfunded calls; reserved total<=approved cap, duplicate attempt reserves once; denial consumes0 attempts; owner publication works. OQ-04 approval required to enable AI. | Not Run |
| TC-020 | CON-02 inclusive fallback; EC-14 [S/A] | 1. Use owner venue with no social account. 2. Disable all external sources/AI. 3. Complete TC-005A/012A. | Owner can submit without social login/post; eligible deal visible<=60 s; source says Submitted by venue. | Not Run |
| TC-021 | CON-03 food/minimal data [I/S] | 1. Browse fresh/declined session; select food. 2. Inspect UI/API/session schema and recorded data. | Food filter prominent; exact eligible food set,0 alcohol; no DOB/ID field, upload, or stored value; only age answer stored. | Not Run |
| TC-022 | CON-04 permitted scraping; EC-14 [I/A] | 1. Attempt unapproved URL, then approved local stub returning429 with Retry-After60,403, and500. 2. Advance clock and inspect calls. 3. Review one permitted live fetch per enabled adapter. | 0 unapproved calls; no429 retry before60 s (15min fallback if absent);403 stops adapter;500 retries next permitted poll; no bypass; missing live-access evidence blocks enablement. | Not Run |
| TC-023 | CON-05 DB isolation [I/S] | 1. Inspect deployment identities/env references. 2. From C1/C2/C5 attempt direct C4 connection; call C3 instead. 3. Review migration route. | Only C3 has application DB credentials/network access; all direct client/worker probes denied; I1-I3 work; migrations use controlled C3 deployment identity. | Not Run |

<!-- pdf-page-break -->

## 5. Test environment

Section 4's sample datasets are called **fixtures**. **Reset** means reload a fresh copy. **T0** is November 4, 2026, noon Eastern (17:00 UTC). Test software/data still need to be built.

**Where tests run**

| Environment | Setup and purpose |
| --- | --- |
| Development | Local Flask/Jinja/FastAPI/Pydantic, PostgreSQL matching staging, pytest, browser tools and axe-core. Separate test database; simulated source/AI/login services. Live scraping/paid calls default off. |
| Staging | Azure web/API/worker containers, managed PostgreSQL, HTTPS, production WSGI server, Supabase test accounts. Proposed: one API instance (2 vCPU/4 GiB), one web instance, one worker. Record database tier; fix resources during timing. Funding/approval required; no production data. |
| Pilot | Invited students/owners use the approved staging build after Must tests pass. Record devices/browsers and setup differences. AI stays off unless approved. |

**Record the setup:** at S01, Meredith/Ved save hardware, exact software/container/migration/timezone versions, code/data versions, dataset checksum and nonsecret settings. Missing details block affected tests. Draft reference: Windows x64 25H2 build 26200.9711, Edge 156.0.4314.58; proposed Python 3.12/PostgreSQL 16. Confirm versions at S01. Use >=4 logical cores/8 GB RAM, keyboard, NVDA screen reader and 390x844 mobile emulation; emulation cannot prove physical-device compatibility.

**Before each test:** reload/check the dataset; set server/worker clocks; reset sessions, budgets, counters and simulated services. Save requests/responses, screens and database changes. Clock/failure controls are test-only. Use `/api/v1` (worker: `/api/v1/internal` with service key), agreed field names and valid CSRF tokens except negative tests. At S01, document commands to run cases by ID.

**F - sample feed:** “Reset F” loads the whole dataset; “deal F” is its alcohol offer. Use a fictional campus at (0,0), approved venues, invented details and matching source records. Default distance: 1,000 m; last checked: T0.

| Deal ID | Source/type | Eastern schedule | Price / special condition |
| --- | --- | --- | --- |
| A | Owner / food | Wed 12:00-13:00 | $5 |
| G | Owner / food | Wed 12:00-13:00 | $6; exactly 1,609.344 m away |
| B | Automated / food | Wed 18:00-20:00 | $7; original post on simulated HTTPS source |
| F | Automated / alcohol drink | Wed 20:00-22:00 | $3 |
| C | Owner / food | Tue 23:00-Wed 02:00 | $4; final allowed start November 3 |
| D | Otherwise eligible deal | Overlaps selected day | 1,609.345 m away; excluded |
| E | Otherwise eligible deal | Overlaps selected day | Pending review; excluded |

For TC-001A, confirm age and request `include_alcohol=true`: **C,A,G,B,F**. Normal nonalcohol feed: **C,A,G,B**. IDs put A before G. Last update = newest known check across all matching deals; stale count uses that full set.

**Other labels used in Section 4**

| Label | Meaning and data to use |
| --- | --- |
| R - radius | Distances: 1,000 / 1,609.343 / 1,609.344 / 1,609.345 m. Include first three; exclude last. Use exact numbers in unit tests; independently check integration coordinates within 0.1 mm, plus +/-1 cm trials. Real coordinates need OQ-02 approval. |
| DST - daylight saving time | New York clock changes: March 14 / November 7, 2027. Set clock to the selected day to stay within the 14-day query limit. |
| O - owner submissions | Owner OA owns only venue VA; owner OB owns VB. Include nonowner/expired/revoked sessions; K is a UUID submission key. Form: matching business name/address/email, description, category, weekdays/times, alcohol=false; Wed 12:00-13:00. Timing requires currently eligible offers. |
| P - performance | Repeatable 50 venues/300 published deals: both sources, food/alcohol, stale/overnight. Record historical volume; check an independently sorted CSV. Run 100 visits, 5 at once: 10 Mbps down/1 Mbps up/100 ms round-trip delay, no packet loss. Disable browser cache, warm services once, fix data/resources, save settings/timings. OQ-07 approval pending. |
| H - AI accuracy | Kiki prepares 100 unseen approved/anonymized or synthetic posts: 20 no-deal, 20 multi-offer, 20 unclear/missing facts, 40 single clear offers; include food/alcohol/flyers/overnight/reposts. Retain text/images/timestamps, source hashes, expected 0-10 offers. Kaustubh/Kiki independently agree answers; hide them from AI. Fix model/prompt/source format and supported temperature/seed; one call/post. Record cost/variation, correct published deals / all published deals, wrong alcohol/time/price, held/rejected/missed offers and coverage. Require >=95% correct published deals, zero wrong alcohol/time/price; publishing nothing fails. Stakeholders review omissions; no minimum offer-discovery rate is approved. Changed model/prompt needs new unseen data/approval. OQ-01/04 gate live access/funding. |

<!-- pdf-page-break -->

## 6. Pass and fail criteria

**Per case:** Pass requires every trial/outcome to meet Section 3 on the frozen environment. Wrong/missing values, disclosure, duplicate/partial commits or breached thresholds are Fail. Blocked means missing prerequisites/decisions; Not Run means no attempt. Neither passes. Retain failures; rerun repaired cases and affected regression. Mocks cannot establish live acceptance.

**Whole plan/release:** 100% Must cases/all five constraints pass; 0 open critical/high defects. Critical: age/credential disclosure or data loss. High: broken browsing/submission, integrity or accessible-task failure. Selected Should cases pass or have a team/advisor exception naming impact, owner and disabled feature. Disabled automation is not tested coverage. Could items stay excluded; lesser defects need accepted dispositions.

**Acceptance blockers:** OQ-03: optional price versus AC-01.1 completeness. OQ-07:95/100 versus every-load<3 s, and specified 60-second versus proposed 30-second refresh within end-to-end 60 s. Record both latency counts and actual refresh/visibility results. OQ-01/04 gate live automation; OQ-02 real geography; OQ-06 constraint/plan reconciliation. October 30 review dates have passed without closure evidence. Technical tests may run, but affected acceptance/sign-off requires documented resolution; do not infer approval or weaken targets.

**Evidence:** record case/requirement IDs, date/operator, commit/manifest/seed hashes, expected/actual values, status, links, defect/severity, reviewer and retest. Planned path: `test-evidence/<milestone>/<run-id>/` for JUnit, screenshots, timing CSV/HAR, axe/manual/DB/AI reports and decisions. Remove secrets/contact emails/unapproved material. No results exist yet.

## 7. Test schedule

Dates/IDs follow [Week 8 Planning](Planning/Planning.pdf); spring gates are proposed. Owners review cases; Kaustubh coordinates evidence, Ved release review. Activities remain planned.

| Milestone / date | Test activity / Week 8 task linkage | Owner | Required evidence |
| --- | --- | --- | --- |
| F08 Nov 4, 2026 | Draft scope, metrics and cases; task 02. Swap two cases with another team before freezing. | Kaustubh; all owners | Plan/PDF, independent execution feedback or pending action |
| F10/F11 Nov 18; F12 Dec 2 | Review plan/DoD with advisor; five-test triage before presentation; carry plan into design-report appendix. | All; Kaustubh coordinates | Advisor decisions, open issues, linked report appendix; no invented execution |
| S00 Jan 11; S01 Jan 25, 2027 | During tasks 06-10: unit validators/time/age first; real-DB contracts, fixture manifest, UI/form stubs; continuous commit/PR checks. | Meredith/Ved; each owner | Locked manifest, seed/oracle hashes, unit/JUnit and I1-I4 reports |
| S02 Feb 9 | Tasks 11/14/16: auth, lease crash/retry, budget, staging isolation/health. | Ved/Kiki/Meredith | TC-002/006/013/015/019/023 fault traces and row counts |
| S03 Feb 24 | Tasks 12/13/18: manual vertical slice and H evaluation; automation stays gated. | Brian/Kaustubh/Kiki | TC-003/005/007/012/020 results, source/quality evidence |
| S04 Mar 10; S05 Mar 24 | Tasks 15/17-20: system boundaries,100-load timing, axe/keyboard; defect corrections and regression before risk review. | Ved/Brian; all owners | TC-001/004/008/011/014 results; defect/retest log |
| S06 Apr 5; S07 Apr 12 | Tasks 21-23: restore, regression, student/owner/assistive walkthroughs before acceptance review. | Meredith/Kaustubh; Ved | TC-016 restore report; stakeholder task records, signed acceptance or blockers |
| S08 Apr 19; S09 Apr 28; S10 Apr 30 | Tasks 24/25: automation decision, frozen release regression, second-operator handover and final demo. | Kiki; Meredith; all | Funded/access/AI approvals or explicit exception, final matrix, runbook and sign-off |

**Proposed five-test triage card:** likelihood/impact1-3; rank their product, then core flow. Run before demos; full acceptance is still required.

| Run order / case | Requirement | Check | Likelihood x impact = score |
| --- | --- | --- | --- |
| 1 / TC-002B | REQ-02 | Forged age cannot expose alcohol | 3 x 3 =9 |
| 2 / TC-005C | REQ-05 | Cross-venue/DB failure commits nothing | 3 x 3 =9 |
| 3 / TC-001A | REQ-01 | Complete today's set and order | 3 x 3 =9 |
| 4 / TC-012B | NFR-02 | Open feed sees submission<=60 s | 3 x 3 =9 |
| 5 / TC-014C | NFR-04 | Keyboard/reader completes core tasks | 2 x 3 =6 |

TC-002B is done when forgery exposes zero alcohol, valid confirmation permits the offer, and a reviewer reproduces the evidence.

<!-- pdf-page-break -->

## 8. Definition of done

### Feature checklist

- [ ] Agreed requirement/acceptance IDs, design contract, owner and cases; no unapproved scope.
- [ ] Code/migrations peer-reviewed by another teammate and merged through normal review.
- [ ] Unit/integration checks pass, including boundaries, faults and relevant real-DB checks.
- [ ] System results meet Section 3; UI success reflects committed data and correct authorization.
- [ ] Accessibility/security pass; no open critical/high defect.
- [ ] Reproducible hashes, actual outcomes and sanitized failure/retest evidence saved.
- [ ] Traceability, API docs and user/runbook instructions current; changed IDs logged/mapped.
- [ ] Applicable stakeholder validation recorded; unresolved policies remain explicit.

### Project checklist

- [ ] All nine Must requirements/five constraints satisfy Section 6; selected Should cases pass or have disabled-feature exceptions.
- [ ] Requirements/specification reconcile OQ-02/03/07 acceptance; OQ-06 conflicts resolved.
- [ ] Live automation has OQ-01 access and OQ-04 funding/model/quality approval, or an explicit manual-only exception.
- [ ] Frozen-build regression, restore, logs, HTTPS, least privilege and second-operator handover evidenced.
- [ ] Student/owner/assistive-technology validation recorded; feedback fixed/retested or accepted.
- [ ] No open critical/high defects; other defects/limits/deferred scope have owners and accepted dispositions.
- [ ] QA lead/component owners confirm evidence; advisor records dated accept/revise decision. Submission is not sign-off.
- [ ] Final-report appendix/README link source/PDF/results; all five names included; team owns reviewed AI-assisted metrics/cases/DoD.

**Current state:** all cases Not Run; harness, environment freeze, recruitment, team review and advisor acceptance pending.

### Appendix A. Supporting sources

| Source | Use in this plan |
| --- | --- |
| [Requirements v0.2](Requirements_Document.md), Sections 5-9 | Requirement IDs, priorities, targets, assumptions and open decisions |
| [Technical Specification v0.4](Technical_Specification.md), Sections 2-6 | Interfaces, data formats, limits, failure behavior and test mappings |
| [D0](Design_Diagrams/D0_Detailed_Design.md), [D1](Design_Diagrams/D1_Detailed_Design.md), [D2 v1.2](Design_Diagrams/D2_Detailed_Design.md) | Component ownership, scheduling, transactions, recovery and EC-01-15 |
| [User Stories](User_Stories.md); [Constraints](Constraints.md); [Project Description](Project-Description.md) | Stakeholder needs, economic/social/ethical limits and current scope |
| [Planning Package](Planning/Planning.pdf); [Task List](Planning/Tasklist.md) | Milestone dates, implementation tasks and owners |
| [Peer Review Notes](Design_Diagrams/Peer_Review_Notes.md), PR-01-05 | Coverage, source access, day semantics, references and extraction-quality risks; closure not assumed |

### Appendix B. Deferred Could procedures

These procedures preserve Week 9 IDs and run only after a recorded scope decision.

| ID | Description | Steps | Expected Result | Status |
| --- | --- | --- | --- | --- |
| TC-009 | REQ-09 deferred active mode [U/I] | 1. Only if scope approved, query 12:00-13:00 offer at 11:59:59/12:00/12:59:59/13:00. 2. Compare food-first/minutes/ID ordering. | Absent/present/present/absent; remaining minutes rounded up; exact tie order. Excluded from this cycle's completion claims. | Not Run |
| TC-018 | NFR-08 deferred stretch [S] | 1. Only if approved, seed 5,000 venues/30,000 published/about 5 million historical rows. 2. Repeat TC-011A. | >=95/100 loads<3 s at 5 concurrent with same network; failure does not fail MVP; report actual volume. | Not Run |

**Coverage:** 21 IDs in scope; REQ-09/NFR-08 deferred. All 23 original groups retained:36 major cases (12x3),9 other in-scope cases,2 deferred procedures =47. D2 EC-01-15 are covered.

**Document links:** [Markdown source](Test_Plan.md) and [PDF](Test_Plan.pdf). Team review and ownership of the metrics, cases and definition of done remain pending.
