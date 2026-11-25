# Search Queries for Job Scraper

Candidate: Zain Ul Hassan. Market: Lahore, Pakistan. Hard filters (see `04-job-evaluation.md` "Candidate Hard Filters" and CLAUDE.md Deal-breakers): **Lahore on-site only, minimum PKR 70,000/month (unstated salary = "Unknown – Check"), maximum 2 years' required experience (3+ = reject), no commission-only, no call-centre cold-calling, no MLM, no remote.**

## Installed portal CLIs (primary for `/scrape`)

`/scrape` discovers every portal skill under `.agents/skills/*/SKILL.md` and runs its CLI first. None are installed yet. The Pakistani boards below are not known to have a community portal, so `/scrape` falls back to WebSearch (`site:` queries) for them. Options, in order:
1. Use the WebSearch fallback below (works now, no install).
2. Copy the global LinkedIn portal (`portals/global/linkedin-search` in https://github.com/ai-job-search-dev/community-portals) into `.agents/skills/` after reading its code (needs Bun). Printed prompt for you to paste:
   > Copy https://github.com/ai-job-search-dev/community-portals/tree/main/portals/global/linkedin-search into `.agents/skills/` and show me its code and what it does. Once I confirm, run `python3 tools/install_portal_clis.py`.
3. Generate a portal for Indeed Pakistan, Rozee.pk or Mustakbil with `/add-portal` (it declines auth-walled or bot-blocking sites).

**Language scope:** queries in English only (Urdu and Punjabi postings for BD roles are rare). A posting requiring a language not in the Languages table, as a job condition, is excluded. Postings asking for "fluent" English are flagged, not excluded (English is Intermediate).

## Search Sites

Primary (Pakistan job boards), in priority order:
- **pk.indeed.com** (Indeed Pakistan) - PRIMARY priority
- **rozee.pk** - Pakistan's largest local board
- **mustakbil.com** - local board
- **linkedin.com/jobs** - filter: Lahore, Pakistan; also covered by the community `linkedin-search` CLI once installed

Secondary (company career pages via Google): direct `site:` searches for Lahore software houses, IT services, SaaS, fintech and e-commerce companies.

## Query Categories

Prioritise **actual Lahore vacancies**. Search broadly, then apply the hard filters strictly. Combine each query with "Lahore" where the site supports it.

**Plain terms** blocks are what portal CLIs consume: one term per line, no `OR`, no `site:`, no location. **WebSearch-only** blocks are Google syntax for the fallback path.

### Priority 1: Business Development and Lead Generation (core target)

#### Plain terms (portal CLIs)

```
Business Development Executive
Business Development Representative
BDR
Lead Generation Executive
Client Acquisition
Sales Executive
```

#### WebSearch-only (`site:` fallback)

```
site:pk.indeed.com "Business Development Executive" Lahore
site:pk.indeed.com "Business Development Representative" Lahore
site:pk.indeed.com "Lead Generation" Lahore
site:pk.indeed.com "Client Acquisition" Lahore
site:pk.indeed.com "Sales Executive" Lahore
site:rozee.pk "Business Development Executive" Lahore
site:rozee.pk "Lead Generation" Lahore
site:rozee.pk "Sales Executive" Lahore
site:mustakbil.com "Business Development" Lahore
site:linkedin.com/jobs "Business Development Executive" Lahore Pakistan
site:linkedin.com/jobs "Lead Generation" Lahore Pakistan
```

### Priority 2: B2B / IT Services / Software Sales

Where the software background and LinkedIn Sales Navigator experience are an advantage.

#### Plain terms (portal CLIs)

```
B2B Sales
Sales Navigator
Software Sales
IT Sales
Pre-Sales
```

#### WebSearch-only (`site:` fallback)

```
site:pk.indeed.com "B2B Sales" Lahore
site:pk.indeed.com "LinkedIn Sales Navigator" Lahore
site:pk.indeed.com "Software Sales" OR "IT Sales" Lahore
site:rozee.pk "B2B" "business development" Lahore software
site:linkedin.com/jobs "B2B Sales" Lahore Pakistan
site:linkedin.com/jobs "Sales Navigator" Lahore Pakistan
```

### Priority 3: Adjacent Junior Business Roles

#### Plain terms (portal CLIs)

```
Business Development Associate
Account Executive
Account Manager
Inside Sales
Sales Development Representative
SDR
Growth Executive
Marketing and Sales Executive
Client Relationship Executive
```

#### WebSearch-only (`site:` fallback)

```
site:pk.indeed.com "Account Executive" Lahore
site:pk.indeed.com "Sales Development Representative" Lahore
site:pk.indeed.com "Inside Sales" Lahore
site:rozee.pk "Account Manager" Lahore
site:rozee.pk "Growth Executive" Lahore
site:linkedin.com/jobs "Sales Development Representative" Lahore Pakistan
```

### Priority 4: Broader Sector Searches (wider net)

#### Plain terms (portal CLIs)

```
Business Development Software House
Sales SaaS
Fintech Sales
E-commerce Sales
Fresh Graduate Business Development
```

#### WebSearch-only (`site:` fallback)

```
site:pk.indeed.com "business development" "software house" Lahore
site:pk.indeed.com sales SaaS OR fintech OR e-commerce Lahore
site:rozee.pk "fresh" "business development" Lahore
"business development executive" Lahore hiring software house
```

## Location Filter

Verify every result's location against these tiers:
- **Ideal:** Shalimar Town, Mughalpura, Baghbanpura, Mall Road, Gulberg, Garden Town, Model Town, Johar Town, Faisal Town, Township, Lahore Cantt and other central Lahore areas
- **Acceptable:** any other Lahore area, including DHA Lahore, Ferozepur Road, Raiwind Road (check the commute)
- **Reject:** **Bahria Town and Wapda Town** (user's deal-breaker); any location outside Lahore (Islamabad, Karachi, Faisalabad, etc.); **remote / work from home**
- **Flag:** hybrid with a Lahore office; postings that say only "Lahore" without an area (acceptable unless the area turns out to be Bahria or Wapda Town)

## Hard Filters to Apply to Every Result

Apply these during `/scrape`'s quick fit assessment and again in `/rank` (full rules in `04-job-evaluation.md`):
- **Salary:** < PKR 70,000/month = reject; not stated = **"Unknown – Check"** (keep, never reject for lack of salary)
- **Experience:** requires 3+ years = reject; "2-3 years" = flag; fresher, 0-2 years or unstated = pass
- **Reject** commission-only, call-centre-style cold-calling / telesales, MLM or network marketing, unpaid trials
- Do not count the Techfy.io backend role as BD experience

## Language Filter

Working languages and levels are in CLAUDE.md's Languages table (Urdu native, English intermediate, Punjabi conversational). Apply `04-job-evaluation.md`'s Language Gate: a posting requiring an undeclared language as a job condition is excluded; "fluent English" is a FLAG shown to the user. Postings simply written in English are fine.

## Date Filter

Only include jobs posted within the last 14 days, matching `/scrape` Step 1b.3: use a supported recency filter or filter the returned publication dates client-side. A future application deadline does not bypass this discovery window; `/rank` handles deadline expiry and urgency separately after discovery. If a posting date cannot be determined, include it but flag as "date unknown".

## Adapting Queries

If the user specifies a focus area, select queries from the matching category and also generate 2-3 custom queries for that focus. For example:
- "/scrape [focus_area]" -> relevant category queries + custom focus-specific queries

Never submit applications, message recruiters, or push to git without the user's explicit approval.
