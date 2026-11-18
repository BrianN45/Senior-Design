# BarCat Deals: Advisor Feedback

**Course:** CS 5001 - Senior Design I, University of Cincinnati  
**Team:** Brian Nguyen, Kaustubh Mathur, Ved Sanap, Meredith Bartel, Kiki Vasilev  
**Advisor:** Dr. Hrishikesh Vinayak Bhide  
**Document version:** 0.1 | **Prepared:** November 18, 2026

## 1. Meeting record

- **Review date:** November 16, 2026, provided by Brian Nguyen.
- **Format:** Email feedback, documented by the supplied screenshot. No separate meeting is documented.
- **Duration:** Not recorded; the evidence is an asynchronous email.
- **Documented participants:** Dr. Bhide (sender) and Brian Nguyen (recipient). Other team members are listed above but are not shown as recipients or meeting attendees.
- **Reviewed version:** The supplied 11-slide `Senior-Design-Presentation.pptx`, preserved unchanged as [Reviewed_Presentation.pptx](Evidence/Reviewed_Presentation.pptx), including its existing media. No version number or Git commit hash for that reviewed file was supplied.
- **Reviewed file SHA-256:** `87b2573ce18693360d8a116dcf104f32295c6e582f6125bd50430bd8598a51cf`.
- **Evidence:** [Advisor email screenshot](Evidence/Advisor_Email_2026-11-16.png). Its displayed message reads: "Hi Brian, The slides look very good. All the best for the presentation! Best, Hrishi."
- **Submission version:** [Presentation_Draft.pptx](Presentation_Draft.pptx), revised November 18, 2026, with 10 main slides and 6 reference backups. These subsequent changes have not been reviewed by the advisor.

## 2. Feedback log

| ID | Slides affected | Advisor comment | Response and reason | Change made | Owner | Status |
| --- | --- | --- | --- | --- | --- | --- |
| AF-01 | Supplied deck overall, slides 1-11 | "The slides look very good." The message also wishes the team well for the presentation. | Accepted. Retain the project narrative and record the positive feedback. The advisor requested no specific revisions. | Preserved the original reviewed deck and email evidence. Subsequent assignment and consistency edits are documented separately below. | Brian Nguyen | Feedback documented; no advisor-requested correction outstanding. |

This is the complete feedback supplied. The email does not establish a 30-minute review session, 5-10 substantive recommendations, technical approval of every design decision, or approval of the revised deck. No additional comments have been inferred or invented.

## 3. Summary of changes

The submission deck consolidates the original 11 main slides into 10 by combining the block diagram and system-design explanation, while preserving the required topic order. It shortens the main-slide wording, redraws D0 as editable components, and includes the full existing D1/D2 diagram images in reference backups with copies of the complete D1/D2 PDFs. The division-of-work table now follows the project task list: Kaustubh owns the owner portal, and Ved owns the backend API and authentication. Speaker notes allocate 600 seconds across the ten main slides, with 120 seconds per member, and a backup slide records the proposed recording and Expo fallback plan. A final backup discloses Codex assistance; these preparation changes respond to assignment requirements and project-document consistency, rather than to revision requests from the advisor.

### Proposed slide outline and timing

These are proposed presentation roles, not evidence that members have already agreed or rehearsed.

| Slide | Title | Content cues | Visual | Speaker | Seconds |
| --- | --- | --- | --- | --- | --- |
| 1 | BarCat Deals | Purpose, feed, owner submissions, conditional automation | Purpose and goal statements | Brian Nguyen | 30 |
| 2 | Team and Advisor | All names, emails, advisor | Contact list | Kaustubh Mathur | 40 |
| 3 | Project Abstract | Radius, freshness, age gate, sources, planned architecture | Abstract with highlighted key phrases | Meredith Bartel | 70 |
| 4 | User Stories | Adult student, under-21 student, owner, accessible browsing | Four stakeholder stories | Brian Nguyen | 50 |
| 5 | System Design: D0 Overview | C1/C2/C5 call C3; C3 alone accesses C4; C6 supplies posts | Editable D0 diagram | Ved Sanap | 120 |
| 6 | Major Project Constraints | Spending, fair access, alcohol, source permission | Four constraint statements | Kiki Vasilev | 60 |
| 7 | Project Progress | Documents prepared; implementation/tests remain; open decisions | Progress statements | Meredith Bartel | 50 |
| 8 | Expected End-of-Term Accomplishments | Reviews, report, open decisions, spring preparation | Planned accomplishments | Kiki Vasilev | 60 |
| 9 | Division of Work | Component owners and supporting duties | Editable ownership table | Kaustubh Mathur | 80 |
| 10 | Expected Expo Demo | Feed, detail, food-only mode, owner publication, fallback | Planned demonstration sequence | Brian Nguyen | 40 |
| 11-16 | Reference backups | Recording plan, D1 model, D2 client/data/state diagrams, sources/AI assistance | Detailed references | Relevant member, only if asked | 0 |
| Total | Main presentation | Brian 120; Kaustubh 120; Ved 120; Meredith 120; Kiki 120 | 10 main slides | All members | 600 |

### Recording and Expo plan

All five members will present according to the proposed outline. Use PowerPoint Record Slide Show, targeting a 10-minute final video. Brian is the proposed owner for exporting MP4, storing the video in UC OneDrive with an instructor-accessible sharing link, and posting the deck and video links to the team spreadsheet in Week 13. A test recording and audio check are planned for November 20, 2026; no completed recording or video URL is claimed here. The original presentation's existing media remain in the preserved reviewed file; the revised deck needs new narration.

The planned Expo demonstration starts with the student feed, opens a deal's source/freshness details, shows that alcohol offers are hidden without 21+ confirmation, and submits a deal through an approved owner's portal. The target is publication in the student feed within 60 seconds, subject to validation against the test plan. If the live application or network fails, play a local MP4 of the same sequence using clearly labeled synthetic fixtures, with local screenshots as a second fallback. Prepare these fallback assets before Expo; they are not existing implementation or test evidence. Keep automation disabled unless access, spending, and extraction-quality gates have passed.

### Sources and AI assistance

The preparation used the supplied presentation, [project description](../Project-Description.md), [requirements](../Requirements_Document.md), [technical specification](../Technical_Specification.md), [user stories](../User_Stories.md), [constraints](../Constraints.md), [task list](../Planning/Tasklist.md), [test plan](../Test_Plan.md), and existing D0/D1/D2 diagrams. Current requirements and the specification take precedence over historical diagram details.

Codex assisted with checking assignment coverage, consolidating slides, drafting concise wording, redrawing the overview, proposing timing/recording roles, documenting the supplied feedback, and generating files. Team verification of the revised deck and proposed speaking assignments remains pending. This record does not claim that the advisor reviewed or endorsed AI-generated revisions.

## 4. Open items

Dates below are preparation targets, not claims of completion.

| Item | Owner | Target close date | Completion condition |
| --- | --- | --- | --- |
| Verify the revised content and confirm speaker/recording roles | All team members; Brian coordinates | November 18, 2026 | Team confirms technical wording, ownership, and timing. |
| Finalize repository submission reference | Brian Nguyen | November 18, 2026 | Commit the Presentation package and include the repository link and actual commit hash in the Canvas comment. The original advisor-reviewed deck has no supplied commit hash; use the preserved file and SHA-256 to distinguish it. |
| Upload both required PDFs to Canvas | Each team member | November 18, 2026 | Submit the draft-deck PDF and feedback PDF separately with all five names. |
| Complete a timed audio rehearsal | All members; Brian coordinates | November 20, 2026 | Check intelligibility, handoffs, and the 600-second limit; adjust timing if needed. |
| Publish final presentation links | Brian Nguyen | November 25, 2026 (Week 13 target; verify course deadline) | Instructor-accessible deck/video links posted to the team spreadsheet. |
| Confirm fallback production milestone | Component owners; Brian coordinates | November 25, 2026 | Record the spring Expo readiness date and owners for the local demonstration MP4 and screenshots. |

The supplied advisor email requests no further changes. If the instructor requires a more substantive feedback session than this evidence provides, that additional review must be obtained and recorded separately; it cannot be reconstructed from the screenshot.
