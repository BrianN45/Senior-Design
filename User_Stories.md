# User Stories and Use Cases: BarCat Deals

**Team members:** Brian Nguyen, Kaustubh Mathur, Ved Sanap, Meredith Bartel, Kiki Vasilev

**Assignment 4, Part 1 (Week 4)**

## Stakeholder Map

| Type | Who | Story |
|---|---|---|
| Primary | UC student, 21 or older | US-01 |
| Primary | UC student, under 21 | US-02 |
| Secondary | Clifton bar or restaurant owner | US-03 |
| Hidden | Student who uses a screen reader or keyboard only | US-04 |

## Brief Use Case Description

UC-01, Browse Tonight's Deals: A UC student opens the app, says whether they are 21 or older, and views the bar and food deals near campus tonight in one feed.

## User Stories

- [x] US-01 (Primary): As a UC student who is 21 or older, I want to see all the bar and food deals near campus tonight in one place so that I can pick where to go in a few minutes instead of checking every bar's social media. *(AC-01.1, AC-01.3)*

- [x] US-02 (Primary): As a UC student under 21 on a tight budget, I want to find food deals near campus without being shown alcohol promotions so that I can save money on meals and not see things I am too young for. *(AC-01.2)*

- [x] US-03 (Secondary): As the owner of a Clifton bar or restaurant that does not post deals much online, I want to send in my venue's deals myself so that students can find my specials and I get customers I would lose to bigger places. *(AC-03.1)*

- [x] US-04 (Hidden, accessibility): As a student who uses a screen reader or only a keyboard, I want to read every deal's details with my assistive technology so that I can plan a night out just like any other student. *(AC-04.1)*

INVEST check: All four stories are Independent (each can be tested with sample deals), Negotiable (none describes a screen or button), Valuable, Estimable, Small, and Testable.

## Use Case

### UC-01: Browse Tonight's Deals

Expands: US-01 (alternate flow A1 covers US-02)

Primary actor: UC student

Secondary actor: Deal ingestion service (the AI scraper), which supplies the deals

Preconditions:

1. The deals database has at least 1 deal that was checked in the last 24 hours.
2. The student can open the app in a web browser.

Main success flow:

1. Student opens the app.
2. System asks if the student is 21 or older.
3. Student says they are 21 or older.
4. System shows all deals happening today near campus, sorted by start time.
5. Student opens one deal.
6. System shows the full details and a link to the original social media post.

Alternate flow A1 (at step 3): student is under 21 or closes the question.

1. System shows only food deals and events without alcohol, and says alcohol deals are hidden.
2. Student continues at step 5.

Exception flow E1 (at step 4): the deal data is old.

1. System finds that the last update was more than 24 hours ago.
2. System still shows the deals, shows the time of the last update, and marks deals older than 24 hours as possibly out of date.

Postcondition: The student has seen deals that match their age answer, and no deal older than 24 hours is shown as up to date.

## Acceptance Criteria

- [x] AC-01.1 (main flow): Given the database has at least 1 deal checked in the last 24 hours and the student said they are 21 or older, when the student opens the app, then the app lists every deal happening today within 1 mile of UC's main campus, earliest start first, in under 3 seconds, and each deal shows the venue, offer, price, time, and when it was last checked.

- [x] AC-01.2 (alternate flow A1): Given the student said they are under 21 or closed the age question, when the student opens the feed, then 0 alcohol deals are shown and every food deal within 1 mile of campus is still listed.

- [x] AC-01.3 (exception flow E1): Given the last update was more than 24 hours ago, when the student opens the app, then a notice with the date and time of the last update is shown above the deals, and every deal last checked more than 24 hours ago is marked as possibly out of date.

- [x] AC-03.1: Given an owner fills in all 7 required items (business name, address, deal description, category, days and times, alcohol yes or no, contact email), when the owner submits the form, then the deal shows up in the student feed within 60 seconds. If any item is empty, 0 deals are saved and the empty item is named in text.

- [x] AC-04.1 (Accessibility): Given the feed page and the deal detail page, when each page is scanned with the axe-core accessibility checker for WCAG 2.1 level AA, then the scan finds 0 critical and 0 serious problems on both pages.
