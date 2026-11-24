# Brian Nguyen · Design Notebook

BarCat Deals · CS 5001 Senior Design I · University of Cincinnati

**Team** · Brian Nguyen, Kaustubh Mathur, Ved Sanap, Meredith Bartel, Kiki Vasilev

**Advisor** · Dr. Hrishikesh Vinayak Bhide

**Design responsibility** · C1 Student Web App and G2 Notifications

We began meeting September 16. Our [team contract](../Team_Contract.md) sets Wednesdays at 3 p.m. in Clifton Court Hall, with 1-2 hours scheduled. We reconstructed the [Wednesday agendas](../README.md#meeting-and-review-records) from commits and project records.

Weeks begin on Mondays. We estimate 2-3 hours per week. Weeks 13-14 are projections.

## Week 01 · Aug 31 · Reconstructed

**Work and evidence.** Our dated project records begin with the [September 8 initial commit](https://github.com/BrianN45/Senior-Design/commit/1a2f28a).

**Decision or next action.** Our first recorded project scope appears in Week 2.

**Hours.** 2-3 h estimated.

## Week 02 · Sep 7 · Reconstructed

**Work and evidence.** I added my professional biography on September 8 ([commit](https://github.com/BrianN45/Senior-Design/commit/f91f528)) and updated the README and roster around September 9. We set up the repository, recorded our roster, and drafted the [project description](../Project-Description.md).

**Decision or next action.** We focused the project on one place to compare nearby specials.

**Hours.** 2-3 h estimated.

## Week 03 · Sep 14 · Reconstructed

**Work and evidence.** I added the [team contract](../Team_Contract.md) and [constraints essay](../Constraints.md) on September 16. We recorded our initial scope, [constraints](../Constraints.md), and Wednesday schedule.

**Wednesday, Sep 16 · Reconstructed agenda.** Set our meeting schedule, clarify the problem of comparing nearby specials, and review economic, social, and ethical constraints. [Team contract commit](https://github.com/BrianN45/Senior-Design/commit/d3b8c56) · [Project scope commit](https://github.com/BrianN45/Senior-Design/commit/ae74784)

**Decision or next action.** We framed the design around economic, social, and ethical constraints.

**Hours.** 2-3 h estimated.

## Week 04 · Sep 21 · Reconstructed

**Work and evidence.** I added the user-story document ([September 23 commit](https://github.com/BrianN45/Senior-Design/commit/4bf820d)). We drafted four [user stories](../User_Stories.md) and a student browsing use case.

**Wednesday, Sep 23 · Reconstructed agenda.** Refine student and owner stories, walk through the browse flow, and define acceptance checks for age, freshness, and accessibility. [User stories commit](https://github.com/BrianN45/Senior-Design/commit/cb9dbd4)

**Decision or next action.** We included age-appropriate browsing, venue-owner submissions, and accessibility in the story baseline.

**Hours.** 2-3 h estimated.

## Week 05 · Sep 28 · Reconstructed

**Work and evidence.** I committed the original D0 architecture and diagrams ([October 2 commit](https://github.com/BrianN45/Senior-Design/commit/53a1aa7)). We drafted the [D0 architecture](../Design_Diagrams/D0_Detailed_Design.md) and I1-I5.

**Wednesday, Sep 30 · Reconstructed agenda.** Sketch our D0 service boundaries and I1-I5, and trace how owner submissions and venue posts enter the feed. [D0 commit, Oct 2](https://github.com/BrianN45/Senior-Design/commit/53a1aa7)

**Decision or next action.** We use separate clients, a backend API, persistent storage, and a background ingestion pipeline.

**Hours.** 2-3 h estimated.

## Week 06 · Oct 5 · Reconstructed

**Work and evidence.** I updated the D1 detailed design ([October 7 commit](https://github.com/BrianN45/Senior-Design/commit/2078264)). We documented the relational model, API contract, and technology choices in [D1](../Design_Diagrams/D1_Detailed_Design.md).

**Wednesday, Oct 7 · Reconstructed agenda.** Review D1's deal model, shared backend contracts, and the choice of PostgreSQL for durable background jobs. [D1 commit](https://github.com/BrianN45/Senior-Design/commit/2078264) · [Backend revisions](https://github.com/BrianN45/Senior-Design/commit/d4aecd8)

**Decision or next action.** We selected Flask, FastAPI, and PostgreSQL, with background jobs backed by the database.

**Hours.** 2-3 h estimated.

## Week 07 · Oct 12 · Reconstructed

**Work and evidence.** I committed D2 and the peer feedback ([October 14 commit](https://github.com/BrianN45/Senior-Design/commit/f2eb33b)). We developed [D2](../Design_Diagrams/D2_Detailed_Design.md) and recorded feedback in the [peer review action log](../Design_Diagrams/Peer_Review_Notes.md#feedback-and-action-items).

**Wednesday, Oct 14 · Reconstructed agenda.** Walk through D2 client and ingestion flows. Record peer review actions on source access, feed behavior, edge cases, references, and AI quality. [D2 commit](https://github.com/BrianN45/Senior-Design/commit/f2eb33b) · [Peer review notes](../Design_Diagrams/Peer_Review_Notes.md)

**Decision or next action.** I took PR-03 on the today-versus-active example.

**Hours.** 2-3 h estimated.

## Week 08 · Oct 19 · Reconstructed

**Work and evidence.** We set October 21 as the follow-up date for the [peer review action log](../Design_Diagrams/Peer_Review_Notes.md#feedback-and-action-items).

**Wednesday, Oct 21 · Reconstructed agenda.** Follow up on our five peer review actions. Check access evidence, the today-versus-active example, and the proposed AI evaluation. [Scheduled review follow-up](../Design_Diagrams/Peer_Review_Notes.md#feedback-and-action-items)

**Decision or next action.** For PR-03, I plan to illustrate later-today and overnight offers in both viewing modes.

**Hours.** 2-3 h estimated.

## Week 09 · Oct 26 · Reconstructed

**Work and evidence.** I committed the requirements and specification reconciliation ([contracts commit](https://github.com/BrianN45/Senior-Design/commit/64f8715)) and updated the project description ([description commit](https://github.com/BrianN45/Senior-Design/commit/978ee3a)). We added the [planning package](../Planning/Planning.pdf), [requirements](../Requirements_Document.md), and [technical specification](../Technical_Specification.md).

**Wednesday, Oct 28 · Reconstructed agenda.** Align requirement IDs and interfaces, assign tasks and milestones, and record owners for open decisions. [Requirements and spec commit](https://github.com/BrianN45/Senior-Design/commit/38e9256) · [Task list commit](https://github.com/BrianN45/Senior-Design/commit/3e08bf3)

**Decision or next action.** I recorded the constraints and acceptance criteria that affect C1 and G2.

**Hours.** 2-3 h estimated.

## Week 10 · Nov 2 · Reconstructed

**Work and evidence.** I committed the draft test plan and its cross-references ([November 4 commit](https://github.com/BrianN45/Senior-Design/commit/6a4350b)). We drafted the [test plan](../Test_Plan.md) with fixtures, pass criteria, and cases linked to requirements.

**Wednesday, Nov 4 · Reconstructed agenda.** Map tests to requirements and review fixtures for age gating, schedule limits, accessibility, and owner publication timing. [Test plan commit](https://github.com/BrianN45/Senior-Design/commit/6a4350b)

**Decision or next action.** I included keyboard, age, stale-state, and browser timing checks for C1.

**Hours.** 2-3 h estimated.

## Week 11 · Nov 9 · Reconstructed

**Work and evidence.** I committed the ethics statement and repository audit ([November 11 commit](https://github.com/BrianN45/Senior-Design/commit/e33d2ff)). We recorded responsible-use controls in the [ethics statement](../Ethics_Statement.md) and checked repository credentials in the [security audit](../Security_Audit.md).

**Wednesday, Nov 11 · Reconstructed agenda.** Review our responsible-use controls for alcohol offers, data retention, source permission, and credential handling. [Ethics and audit commit](https://github.com/BrianN45/Senior-Design/commit/e33d2ff)

**Decision or next action.** We limit personal data and require approved access, funding, and quality checks before enabling automation.

**Hours.** 2-3 h estimated.

## Week 12 · Nov 16 · Reconstructed

**Work and evidence.** I documented advisor feedback and added the presentation run sheet ([run sheet commit](https://github.com/BrianN45/Senior-Design/commit/1a6d03a)). We prepared the [slides](../Presentation/Senior-Design-Presentation.pptx), [run sheet](../Presentation/Run_Sheet.md), and [advisor feedback record](../Presentation/Advisor_Feedback.md). My assigned section was slides 1-2 on the problem, goals, team, and advisor.

**Wednesday, Nov 18 · Reconstructed agenda.** Check our slide order, speaking handoffs, timing, and anticipated questions. Record advisor feedback. [Run sheet commit](https://github.com/BrianN45/Senior-Design/commit/1a6d03a) · [Advisor feedback](../Presentation/Advisor_Feedback.md)

**Decision or next action.** We assigned slide order, speaking handoffs, timing, and anticipated questions.

**Hours.** 2-3 h estimated.

## Week 13 · Nov 23 · In progress November 23

**Work and evidence.** We updated our [design report](../README.md) and notebooks with the Wednesday agendas, weekly effort estimates, and links to the full contribution records. Our [Budget](../README.md#9-budget) records zero expenses, donated items, or credits as of November 23.

**Wednesday, Nov 25 · Planned.** Compare our report and notebook entries with the repository and identify missing effort evidence. [Design notebook evidence](../README.md#individual-effort-evidence)

**Decision or next action.** We plan to use a Claude plan and have yet to set a budget. We also need to verify campus and venue coordinates.

**Hours.** 2-3 h projected.

## Week 14 · Nov 30 · Planned

**Work and evidence.** We plan to review our [design report](../README.md) and individual submission materials for December 2.

**Wednesday, Dec 2 · Planned.** Review the report, confirm individual submission materials, and check the status of the signed advisor form. [Report contents](../README.md#contents)

**Decision or next action.** We plan to check the report links, review the notebooks, and check the status of the signed advisor form.

**Hours.** 2-3 h projected.

## Contribution summary

**Total hours.** 28-42 h estimated across 14 weeks.

My contributions cover project setup, initial stories, D0/D1/D2, requirements and testing documents, ethics, and presentation coordination. I own the student experience and inline feedback. We need the campus coordinates and browser timing and accessibility evidence for acceptance. See our [shared contribution records](../README.md#individual-effort-evidence).
