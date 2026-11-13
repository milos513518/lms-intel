# Agent instructions: lms-intel

You are working a target list of companies that share the anchor deal's developer education architecture, detected from public signals. Read this file fully before touching anything.

## Source of truth

`targets.csv` is the record. Columns:

```
company,tier,lms,score,status,owner,owner_title,timing_signal,opener_angle,academy_host,platform_age_yrs
```

* `status` is the work lock: Not started | Drafted | Sent | Replied | Meeting | In deal | Disqualified. Claim a row by changing its status before working it. Never work a row another agent has already moved past Not started.
* `score` is the 100 point ICP score (rubric in `docs/reverse-engineering-map.md`). 55+ tier one, 30 to 54 nurture, under 30 waits for a timing signal.
* `tier` groups by detected LMS: 1 Creator (Thinkific/LearnWorlds/Podia), WorkRamp, 2 Skilljar, 3 Docebo, Thought Ind., Other LMS, Legacy, Graveyard, Greenfield.

## Jobs, in priority order

1. **Owner gaps.** Rows where owner is TBD, OPEN, None found, or IN FLUX need a people pull: LinkedIn search "[company] customer education OR technical enablement OR training". Write name and exact title. Verify the person is current; departed owners go in timing_signal as churn evidence instead.
2. **Timing layer.** Rows with empty timing_signal: check hiring pages for education/enablement roles, recent leadership changes, platform age vs the 3.5 to 5.5 year renewal window (signal 7), and migration chatter. Write what you find, dated.
3. **Freshness sweep.** Quarterly, re-run the 9 detection signals per `docs/reverse-engineering-map.md` against each academy_host. Academies migrate, die, and relaunch; update lms, score, and tier accordingly.
4. **Outreach support.** For rows moving to Drafted: the opener_angle column is the story, the research informs question wording only, and no message ever opens with a research fact.

## Hard rules

* Public signals only. Never invent a name, title, or email. Unverifiable fields stay empty.
* Every change to a row should be committable with a one line reason (e.g. "CircleCI VP seated, status to Sent").
* Dead or acquired companies get Disqualified with the reason in timing_signal, never deleted. History is evidence.
* Do not touch rows with status In deal.
* All work is attributed to Ranch Lab.

## Waymo track

The `waymo/` directory holds the second motion: internal engineering onboarding rather than customer education. Its playbook is people-first (see `waymo/detection-signals.md`); no DNS or page signals exist for internal programs. Rules here carry over.

5. **Waymo lookalike sweep.** `waymo/targets.csv` holds the schema and the anchor row only. Build the candidate pool from companies with engineering headcount growth above ~25% YoY plus new international hubs, then score against the 7 signals in `waymo/detection-signals.md`. The trigger signal is an enablement hire from a big-tech program within their first 90 days (LinkedIn saved search on role changes). Append rows with public evidence per column; this repo is public, so roles and facts, not per-person dossiers.
