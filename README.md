# lms-intel

A Ranch Lab pipeline that finds companies with the same unsolved developer education problems as Datadog, using only public signals. The anchor deal's pains were reverse engineered from call notes and the follow up email, turned into a detection signature, and scored across 266 dev-facing companies. Output: **75 targets** (1 anchor + 74 lookalikes), most with a named owner and a timing signal.

Built to be worked by agents. `AGENTS.md` holds the operating instructions. `targets.csv` is the record of truth for the list.

## The process

```
anchor deal pains  →  9 public detection signals  →  score 266 companies  →  75 targets  →  owner + timing  →  outreach
```

### The 9 detection signals

All checkable from the outside in minutes, detail and detection method per signal in `docs/reverse-engineering-map.md`:

1. **Academy DNS records**: a CNAME on learn/academy/university.company.com names the exact LMS vendor (this is how Thinkific was confirmed behind learn.datadoghq.com)
2. **URL path patterns**: /courses/ and /enroll/ is Thinkific, /course/view is Moodle, and so on, for when DNS is masked
3. **Page rendering**: server rendered academies are visible to Google and AI, JavaScript shell academies (Docebo) are invisible
4. **Course structured data**: JSON-LD Course schema, almost universally absent, so no academy appears in rich results or AI answers
5. **robots.txt AI posture**: mismatched AI crawler rules between academy, docs, and main site means no coherent AI content strategy
6. **llms.txt presence**: docs teams adopt it, LMS vendors cannot serve it, and that split is the pitch
7. **Platform age**: Wayback Machine first capture of the academy host; the 2021 to 2022 launch cohort is hitting 4 to 5 year contract fatigue now
8. **Separate hands-on lab vendor**: Instruqt, Appsembler, or Strigo assets mean completion data lives in two disconnected systems
9. **Education owner on LinkedIn and their churn**: who owns the problem, whether the seat is empty, newly filled, or hiring

### Scoring

Each company gets a 100 point ICP score (full rubric in `docs/reverse-engineering-map.md`). 55+ is tier one outreach, 30 to 54 is nurture, under 30 needs a timing signal to activate.

### The target breakdown

* **~56 confirmed matches**: verifiably running developer education on a rented LMS right now (creator grade platforms, WorkRamp, Skilljar, Docebo, Thought Industries, and other LMS tiers)
* **7 graveyard**: academies that died, a different pitch ("what killed your education program")
* **5 greenfield**: Instruqt lab customers with no LMS at all, adjacent buyers

The signature proves shared architecture, which means shared ceilings. The timing signals (hiring exhaust, org churn, renewal windows) and the outreach itself convert "matches the signature" into "confirmed pain."

## Repo layout

```
README.md                          this file
AGENTS.md                          operating instructions for agents working this repo
targets.csv                        record of truth: 75 companies, owners, scores, status
docs/reverse-engineering-map.md    the 9 signals in full, detection methods, 100 point ICP rubric
waymo/                             (coming) same process for internal engineering onboarding;
                                   different detection points, same structure
```

## How to work this repo

1. Read `AGENTS.md`.
2. Work `targets.csv` top down by score and timing signal. Status column is the lock: claim a row by moving it from `Not started` before working it.
3. Fill gaps: companies with `TBD`, `OPEN`, or `None found` owners need a people pull; rows without timing signals need the timing layer run (signals 7 and 9).
4. Keep the list fresh: re-run the detection signals quarterly, academies migrate and owners churn.

## Hard rules

* Public signals only. No invented names, titles, or emails: a field that cannot be verified stays empty.
* Never open outreach with a research fact. Research informs the question wording only.
* Status values: Not started | Drafted | Sent | Replied | Meeting | In deal | Disqualified.
* All work is attributed to Ranch Lab.

## Related

The companion repo `sf-events-gtm` holds the SF events pipeline (Nov to Dec 2026): the people whose companies have speakers at Bay Area events, worked off the same system.
