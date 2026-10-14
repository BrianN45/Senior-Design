# BarCat Deals: Peer Review Notes

**Team Members:** Brian Nguyen, Kaustubh Mathur, Ved Sanap, Meredith Bartel, Kiki Vasilev  
**Course:** CS 5001 Senior Design I  
**Assignment:** Design D2 - Peer Design Review Notes  
**Date:** October 14, 2026  
**Document Version:** 1.2

---

# Peer review

**Reviewing team:** Team Agri-AI (Andrea Dall'Ara, Bridget Trout, Dang Luu, and Cyrus Speicher).

## Review Criteria

We used the four criteria from the Week 07 lecture, slides 28-31:

* **Completeness:** Does every component have a data model, behavior, and interface? Are error cases covered, and does every requirement connect to the design?
* **Feasibility:** Can our team build this during the semester with the tools, data, and API access we actually have?
* **Clarity:** Could someone outside our team follow the diagrams and build against the interfaces without guessing?
* **Risk:** Have we explained what is most likely to fail and what the rest of the system does when it happens?

The notes below follow Assignment 7, Part 6. Each item includes the criterion, the reviewer who raised it, a specific action, an owner, a priority, and a due date. All items are open. Follow-up actions are scheduled for our next Wednesday meeting on October 21, 2026.

## Feedback and Action Items

| ID, criterion, and reviewer | Issue and action | Owner, priority, and due date |
| --- | --- | --- |
| PR-01; Completeness; Andrea Dall'Ara | The edge-case table covers important problems, but it is hard to tell whether every component has all four categories. Add a component-by-category check for C1-C6 and G1-G4. Link each cell to an EC row or a specific behavior in Section 2. Add a row for any missing case, including a named handler and whether recovery is possible. | Ved Sanap; must-fix; October 21, 2026. |
| PR-02; Feasibility; Bridget Trout | Reading venue posts depends on access that we have not confirmed. For OQ-01, record which adapters we can actually use and show one permitted account fetch for each enabled adapter. If access is unavailable, keep that adapter off and show that an owner-only venue can still submit and display deals. | Kiki Vasilev; must-fix; October 21, 2026. |
| PR-03; Clarity; Dang Luu | The difference between today's deals and deals active right now takes a while to understand. Add one short example to Section 2.5 showing an offer that starts later today and an overnight offer. State whether each appears in today mode and active mode, and show the matching query. | Brian Nguyen; worth considering; October 21, 2026. |
| PR-04; Clarity; Cyrus Speicher | Sections 2.4 and 2.8 refer to T1, T2, and T3, but there is no reference list explaining those labels. Add a technical references subsection with a source title and link for each label, then check that each citation supports the sentence using it. | Meredith Bartel; must-fix; October 21, 2026. |
| PR-05; Risk; Bridget Trout | A wrong AI extraction could publish the wrong time, price, or alcohol flag. OQ-04 says publication stays off until evaluation passes, but the pass criteria are still open. Write down an agreed evaluation checklist, include ambiguous and missing-field examples, and record who approves enabling publication. Keep uncertain results hidden and verify that manual submissions still work while AI publication is off. | Kaustubh Mathur, with Kiki Vasilev for extraction examples; must-fix; October 21, 2026. |

**Priority decisions:** PR-01, PR-02, PR-04, and PR-05 are must-fix because they affect assignment coverage, usable data access, understandable evidence, or safe publication. PR-03 is worth considering because the rules are already specified, but an example would make them easier to follow. No item is marked out of scope for now. If we defer a suggestion, we will keep it in the log and explain why.

## Feedback Summary and Design Changes

**Clarity** received the most feedback: two items. Completeness, feasibility, and risk each received one. The main point is that we need to make our existing decisions easier to check and show evidence for the assumptions we still have open.

Our next changes are the edge-case coverage check, the today-versus-active example, and the missing reference list. API access and AI quality also need evidence before automation is enabled. These actions remain open until the assigned owner completes them. The existing manual-submission fallback and default-off AI publication policy stay in place.

**Rationale tracking:** PR-02 relates to DR-08, PR-03 to DR-01, and PR-05 to DR-07 and DR-09. When an action changes one of these decisions, we will mark the rationale entry with its PR ID and explain the change. The action items above have not yet changed the recorded design decisions.
