## Team Name
BarCat Deals

## Team Members
- Brian Nguyen (nguyeb2@mail.uc.edu)
- Kaustubh Mathur (mathurkh@mail.uc.edu)
- Ved Sanap (sanapvs@mail.uc.edu)
- Meredith Bartel (bartelmj@mail.uc.edu)
- Kiki Vasilev (vasilekm@mail.uc.edu)

## Project Topic Area
BarCat Deals is a planned web application that helps University of Cincinnati students compare food and bar offers near the main campus. It combines direct submissions from approved business owners with AI-assisted extraction from approved venue social-media pages. Students browse one feed with deal details, schedules, sources, and freshness information. Automated collection is conditional on approved source access, available funding, and acceptable extraction quality.

## Project Faculty Advisor
Name: Dr. Hrishikesh Vinayak Bhide  
Department: Computer Science  
Contact: bhidehk@ucmail.uc.edu

## Project Abstract
UC students often need to check separate venue accounts and social-media posts to find nearby specials. Differences in how venues describe prices and schedules make offers difficult to compare, and older posts may no longer reflect current information. Businesses that rarely post online also need a way to reach students.

BarCat Deals will organize eligible food and bar offers within one mile of a fixed UC main-campus reference point. The default feed will show today's offers, including those starting later in the day, ordered by start time. Students can open full details, view an original source link or an owner-submission label, and see when each offer was last checked. Missing prices will be disclosed, and information checked more than 24 hours ago will be marked as possibly out of date.

Students can browse without registering. Students must confirm they are 21 or older before viewing alcohol offers. The answer is stored on the server and is self-attestation, not verified age. Approved owners will submit deals through a dedicated portal. A separate background service will web scrape approved venue pages and use AI to extract structured offers when automation is enabled. The project is currently specified in design documents; implementation and acceptance testing remain to be completed.

## Inadequacy of Current Solutions
Checking individual social-media accounts leaves students to combine venue information, interpret schedules, and judge whether a post is still current. It also provides no shared feed that applies the project's campus radius, age eligibility, and freshness rules.

A collection process based only on social-media posts would miss businesses that post infrequently or cannot be accessed by the scraper. Direct owner submissions provide a second source of deals and keep the application useful when automated collection is unavailable. The project therefore combines both sources while making missing information and outdated records visible.

## Technical Background Applicable
The planned implementation uses Python across the web applications, backend, and ingestion worker:

- **Student and owner interfaces:** Flask and Jinja for server-rendered pages, accessible controls, and inline status messages.
- **Backend API:** FastAPI and Pydantic for request validation, eligibility rules, owner authorization, and typed errors. Only the backend accesses the database; the web applications and ingestion worker use its HTTPS/JSON interfaces.
- **Database:** PostgreSQL with SQLAlchemy and Alembic for relational records, transactions, uniqueness constraints, and migrations.
- **Background ingestion:** Site-specific web scrapers, httpx, and APScheduler for collection and polling, with the Anthropic SDK for AI extraction. Durable work records, leases, retries, and duplicate matching support recovery without publishing unchecked output.
- **Identity and sessions:** Supabase Auth for verified owner identity, plus application-managed sessions for guest age confirmation and authorization. The application does not collect birth dates or ID images.
- **Hosting and validation:** Separate application containers on Azure with managed PostgreSQL; pytest, browser checks, and axe-core for verification. The exact Azure services, spending limits, source access, and AI-quality approval remain open decisions.

## Team Approach
The team will build the database and shared API contracts first, then connect the student and owner interfaces and the background ingestion service. Owner submissions and representative test data will support early integration. Web scraping and AI publication will remain disabled until the required access, funding, and quality checks pass.

Primary responsibilities follow the existing component ownership:

| Team member | Primary responsibility |
| --- | --- |
| Brian Nguyen | Student Web App and inline notifications/status messages. |
| Kaustubh Mathur | Owner Web Portal, onboarding flow, and submission feedback. |
| Ved Sanap | Backend API Service, authentication, authorization, and eligibility rules. |
| Meredith Bartel | Deal Database, migrations, logging, and health diagnostics. |
| Kiki Vasilev | Deal Ingestion Service, external-source integration, AI extraction, and configuration. |

Testing will cover age restrictions, distance and schedule boundaries, missing fields, duplicate requests, failed dependencies, accessibility, and recovery. Performance validation will check the initial feed target of under three seconds and the 60-second visibility target for currently eligible owner submissions; the workload and refresh budget remain subject to review. Bookmarks, a moderation dashboard, and outbound email/SMS/push notifications remain outside the current MVP.

The [Requirements Document](Requirements_Document.md) defines the required behavior and acceptance conditions. The [Technical Specification](Technical_Specification.md) describes the current implementation plan, and the [D0/D1/D2 design documents](Design_Diagrams/) provide supporting diagrams and earlier design detail. Proposed answers in the open-question register remain subject to confirmation.
