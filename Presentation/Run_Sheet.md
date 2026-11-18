# BarCat Deals: Run Sheet and Rehearsal Log

**Team:** Brian Nguyen, Kaustubh Mathur, Ved Sanap, Meredith Bartel, Kiki Vasilev

**Advisor:** Dr. Hrishikesh Vinayak Bhide

**Presentation:** [Senior-Design-Presentation.pptx](Senior-Design-Presentation.pptx) ([PDF](Senior-Design-Presentation.pdf)), 11 slides.

**Working limit:** 10 minutes presenting, followed by 5 minutes of questions; confirm with the instructor. Planned slide times include transitions.

## Speaking Roles and Timekeeping

The speaking order below follows the team's reported November 16 rehearsal. The planned presentation gives each member two minutes.

| Member | Slides | Speaking role | Planned speaking time |
| --- | --- | --- | --- |
| Brian Nguyen | 1-2 | Introduce the problem, project goals, team, and advisor. | 2:00 |
| Kaustubh Mathur | 3-4 | Explain the application overview and user stories. | 2:00 |
| Ved Sanap | 5-6 | Explain the block diagram, components, and technology choices. | 2:00 |
| Meredith Bartel | 7-8 | Explain constraints, project progress, and remaining decisions. | 2:00 |
| Kiki Vasilev | 9-11 | Explain planned accomplishments, division of work, and the Expo demo; invite questions. | 2:00 |
| **Total** | **1-11** | **All five members present.** | **10:00** |

**Planned timekeeper: Meredith Bartel.** Brian Nguyen tracks the clock while Meredith presents slides 7-8. Meredith signals one minute remaining at 9:00 and the presentation limit at 10:00. Brian records section times during the next timed rehearsal. These are planned duties; no timekeeper was identified for the November 16 practice session.

## Slide Run Sheet

Times are elapsed from the start of the talk. A transition to another slide by the same speaker is included as a continuation line.

| Slide | Title | Speaker | Planned start | Planned duration | Hand-off or continuation line |
| --- | --- | --- | --- | --- | --- |
| 1 | BarCat Deals | Brian Nguyen | 0:00 | 1:00 | "First, let me introduce our team and advisor." |
| 2 | Team Information | Brian Nguyen | 1:00 | 1:00 | "Kaustubh will explain what the application does and who it serves." |
| 3 | Project Abstract | Kaustubh Mathur | 2:00 | 1:00 | "These features come from the needs of our students and venue owners." |
| 4 | User Stories | Kaustubh Mathur | 3:00 | 1:00 | "Ved will show how the system supports these users." |
| 5 | Block Diagram | Ved Sanap | 4:00 | 1:00 | "Here are the technologies behind those components." |
| 6 | System Design | Ved Sanap | 5:00 | 1:00 | "Meredith will explain the constraints that shape this design." |
| 7 | Major Project Constraints | Meredith Bartel | 6:00 | 1:00 | "With those constraints in mind, here is our current progress." |
| 8 | Project Progress | Meredith Bartel | 7:00 | 1:00 | "Kiki will cover our next milestones and planned demonstration." |
| 9 | Expected Accomplishments | Kiki Vasilev | 8:00 | 0:40 | "We have divided the work across the main components." |
| 10 | Division of Work | Kiki Vasilev | 8:40 | 0:40 | "Those components come together in our planned Expo demo." |
| 11 | Expected Demo at Expo | Kiki Vasilev | 9:20 | 0:40 | "Thank you. We are ready for your questions." |
| **End** | **Questions** | **Relevant question owner** | **10:00** | **Up to 5:00** | **Kiki directs each question to its owner below.** |

If rehearsal exceeds the limit, shorten explanations and remove repeated details before the next run. Keep the core problem, component boundaries, constraints, progress, and demo sequence.

### Checks Before Presenting

- Use this speaking order when rehearsing; the existing PowerPoint notes contain earlier speaker assignments.
- Confirm slide 10's component labels: the current task list assigns the Owner Web Portal to Kaustubh and the Backend API to Ved. The slide currently swaps those labels.
- Describe implementation, testing, hosting, and Expo behavior as planned work. The advisor's positive slide feedback does not establish a completed test-plan review.
- Keep a local copy of the deck. The proposed Expo fallback is a local demo recording and screenshots; prepare those assets before Expo.

## Anticipated Questions

The named owner answers first. Other members add details only when useful. Two unresolved issues are explicitly identified with next steps.

| No. | Possible question | Owner | Short answer |
| --- | --- | --- | --- |
| 1 | How is this better than checking venue social-media accounts? | Brian Nguyen | It combines nearby offers in one feed, shows sources and freshness, and lets owners submit deals directly. Students can compare offers without checking every account. |
| 2 | What happens if a platform blocks scraping? | Kiki Vasilev | Source access is still unresolved. We will stop collection from denied sources and keep owner submissions available. Next, confirm permitted sources and their access rules before enabling collection. |
| 3 | How will you check the AI's accuracy? | Kiki Vasilev | Compare extracted deals with manually labeled posts and validate required fields. Confirm the model and quality criteria, then document results before enabling automated publication. |
| 4 | Can the application prevent underage users from seeing alcohol deals? | Ved Sanap | The backend hides alcohol offers unless a guest confirms they are 21 or older. This is self-attestation, not verified age; we do not collect birth dates or ID images. |
| 5 | How will you handle outdated or incorrect deals? | Brian Nguyen | Show the source and last-check time, and label information older than 24 hours as possibly outdated. A freshness label does not guarantee that a venue will honor an offer. |
| 6 | Why Flask, FastAPI, and PostgreSQL? | Ved Sanap | Flask/Jinja renders the interfaces, FastAPI applies shared rules, and PostgreSQL stores the records. Only the backend accesses storage, keeping validation consistent across submission paths. |
| 7 | How do you stop an owner from changing another venue's deals? | Kaustubh Mathur | The backend checks the authenticated owner's approved venue mapping on each submission. Signing in does not grant access to every venue. |
| 8 | What will hosting and AI processing cost? | Meredith Bartel | The cost is unresolved. Confirm Azure services, credits, expected request volumes, and a monthly spending cap. Keep automation disabled until funding and limits are approved. |
| 9 | What have you actually completed? | Meredith Bartel | We have prepared the requirements, specification, detailed designs, test plan, and ethics statement. Implementation and acceptance testing remain; document preparation is not evidence of passing tests. |
| 10 | What will Expo show, and what if the live demo fails? | Kiki Vasilev | Show browsing, deal details, food-only results, and an owner submission appearing within the 60-second target. Prepare a local recording and screenshots of the same flow as fallback. |

## Rehearsal Log

### November 16, 2026: Full-Group Practice Session

| Field | Record |
| --- | --- |
| Date | November 16, 2026 |
| Participants | Brian Nguyen, Kaustubh Mathur, Ved Sanap, Meredith Bartel, Kiki Vasilev |
| Coverage | All 11 slides, using the speaking sections listed below |
| Total practice time | Approximately 30 minutes, reported by Brian Nguyen |
| Timing scope | The whole practice session, including pauses and retries; not a measured uninterrupted presentation run |
| Timekeeper during this session | Not recorded |
| Per-section timing basis | The section times below are rough estimates requested by the team, calculated by dividing the approximately 30-minute session equally among five speaking sections. They are not stopwatch measurements. |

| Section | Slides | Speaker | Estimated practice time, including pauses/retries | Planned uninterrupted presentation time |
| --- | --- | --- | --- | --- |
| Introduction and team | 1-2 | Brian Nguyen | Approximately 6:00 | 2:00 |
| Overview and user stories | 3-4 | Kaustubh Mathur | Approximately 6:00 | 2:00 |
| Architecture and technologies | 5-6 | Ved Sanap | Approximately 6:00 | 2:00 |
| Constraints and progress | 7-8 | Meredith Bartel | Approximately 6:00 | 2:00 |
| Milestones, work division, and demo | 9-11 | Kiki Vasilev | Approximately 6:00 | 2:00 |
| **Total** | **1-11** | **All members** | **Approximately 30:00** | **10:00** |

The session established the speaking order. Because presentation-only times were not recorded, the 30-minute practice session cannot establish whether the talk fits the 10-minute limit.

### Next Full-Group Timed Run

**Target:** November 20, 2026, subject to team availability. **Timekeeper:** Meredith Bartel, with Brian covering Meredith's section and recording the results.

Run all 11 slides without restarting and include hand-offs in the clock. Record the actual section times below and their sum; note pauses or technical interruptions separately. Do not replace estimates with measured results until the run occurs.

| Section | Slides | Speaker | Actual section time |
| --- | --- | --- | --- |
| Introduction and team | 1-2 | Brian Nguyen | Not yet recorded |
| Overview and user stories | 3-4 | Kaustubh Mathur | Not yet recorded |
| Architecture and technologies | 5-6 | Ved Sanap | Not yet recorded |
| Constraints and progress | 7-8 | Meredith Bartel | Not yet recorded |
| Milestones, work division, and demo | 9-11 | Kiki Vasilev | Not yet recorded |
| **Total presentation time** | **1-11** | **All members** | **Not yet recorded** |

After the run, enter the date, all attendees, actual total and section times, and any cuts or hand-off adjustments. The measured full-group timed-rehearsal requirement remains open until these results are recorded.
