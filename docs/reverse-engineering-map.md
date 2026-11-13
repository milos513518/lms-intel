# Reverse engineering map: the 9 detection signals

How the anchor deal's pains were turned into an outside-in detection signature. Each signal is checkable in minutes with public tools.

| # | Signal | Detection method | What it tells you |
|---|---|---|---|
| 1 | Academy DNS records | Query learn/academy/university.company.com against public DNS | A CNAME pointing at thinkific.com, skilljarapp.com, or docebosaas.com names the LMS vendor with certainty. This is how Thinkific was confirmed behind learn.datadoghq.com. |
| 2 | URL path patterns | Inspect the academy's route shapes | When DNS is masked, paths give it away: /courses/ and /enroll/ is Thinkific, /course/view is Moodle. This is how Datadog's pre-Thinkific stack was dated as self-hosted Moodle from archived pages. |
| 3 | Page rendering | Fetch the academy as a crawler | Server rendered academies (Skilljar, Thinkific) are visible to search engines. Docebo serves a JavaScript shell that says "Loading" to any crawler, so the whole academy is invisible to Google and AI. |
| 4 | Course structured data | Check for JSON-LD Course schema on course pages | Almost no academy marks up its courses, so none can appear in rich results or AI answers. Universal gap, even at companies spending on SEO. |
| 5 | robots.txt AI posture | Compare academy, docs site, and main site rules for GPTBot and friends | Mismatched rules mean no coherent AI content strategy. |
| 6 | llms.txt presence | Fetch /llms.txt on docs site and academy | Docs teams adopt it, LMS vendors do not support it. Datadog's docs site serves it, their academy cannot. That split is the pitch. |
| 7 | Platform age | Wayback CDX API first capture of the academy host | Academies launched 2021 to 2022 are hitting 4 to 5 year contract fatigue now. Datadog's own history: Moodle until 2019, Instruqt labs Jan 2022, Thinkific Jul 2022, shopping again late 2026. Roughly a 4.5 year half-life. |
| 8 | Lab platform presence | Page assets referencing instruqt.com, appsembler, strigo.io | A separate lab vendor means the LMS cannot do hands-on labs, and completion data lives in two systems that do not talk. Instruqt plus any creator LMS is the exact Datadog pattern. |
| 9 | The owner and their churn | LinkedIn search: "[company] customer education OR technical enablement OR training" | Who owns the problem, whether the seat is empty (orphaned academy), newly filled (rethink window), or hiring (budget approved). Ex-employees describe the stack freely in their profiles. |

## 100-point ICP score

Start at 0, add:

* +20 creator-grade LMS (Thinkific, LearnWorlds, Podia) at a developer-product company
* +15 WorkRamp or other internal LMS doing customer education
* +10 Skilljar, Docebo, Thought Industries, Intellum (purpose-built but gapped)
* +15 separate hands-on-lab vendor detected (Instruqt, Appsembler, Strigo)
* +10 JS-shell academy (invisible to search and AI)
* +10 zero JSON-LD on course pages
* +10 docs site serves llms.txt or .md pages while academy does not
* +10 platform age 3.5 to 5.5 years (renewal window)
* +10 live timing signal (hiring for education roles, new education leader, migration in progress)
* −10 academy retired or company mid-acquisition (different conversation, not zero)

55+ is tier-one outreach. 30 to 54 is nurture. Under 30 needs a timing signal to activate.

## Why this works

The LMS is the one part of a company's stack that must be publicly reachable (customers log into it), publicly named (DNS does not lie), and publicly dated (the Wayback Machine saw it launch). Teams hide their data warehouse, they cannot hide their academy.
