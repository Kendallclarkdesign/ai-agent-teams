# Towpath Creative — Lead-Gen Team Reference

Shared source of truth for the lead-gen agent team (Lead Gen Project Manager, Local Lead Researcher, Lead Verifier, Outreach Copywriter, Lead Queue Data Manager). Read this before doing any lead-gen work. If any agent-specific instructions conflict with this file, this file wins — it's the one place these facts get updated.

## Who Towpath Creative is
- Kendall's own web design/dev agency, based in Frenchtown, NJ.
- Phone: (555) 010-0000
- Emails: you@yourdomain.com, partner@yourdomain.com
- Live site: https://towpathcreative.com/
- Real proof point (use this, don't invent others): built a 5-page site plus local SEO for Nina's Cleaning Service, now at a 4.9-star Google rating.
- Differentiators: month-to-month, direct access to the person doing the work, local knowledge, 1-business-day response.

## Voice rules (non-negotiable, applies to every piece of outreach copy)
- No em dashes, anywhere, ever.
- No filler words: seamless, leverage, robust, cutting-edge, game-changer, unlock, delve, elevate, unleash, next-gen.
- Active voice, short sentences, plain language.
- No generic AI-sounding copy.
- **Tone (added 2026-09-17, Kendall's explicit direction): professional and down-to-earth.** Write like a competent person talking to another business owner, not a marketer pitching a service. Confident, not hype-y. No corporate jargon, no fake urgency, no exclamation points.
- **Angle: lead with the money, not just the gap.** Don't just point out the technical problem (no website / broken site / no online ordering) as an abstract fact. Say what it's actually costing them: customers who search and find nothing, walk past and never come back, or want to order and can't. The technical problem is the setup; the revenue they're leaving on the table because of it is the point. The reader should think "this is costing me real money" before they get to the ask. Still only one concrete, specific observation per email (never generic), the money framing sits on top of that real evidence, it doesn't replace it.
- **Vary it, don't mail-merge it (added 2026-09-28, Kendall's explicit direction: messages were reading too generic/templated).** Every message follows the same rules above, but two messages read back to back should not sound like the same template with the name swapped. Concretely, across a batch:
  - Vary the opener. Not always "I looked up X and...". Try leading with the observation itself, a question, or a direct statement about the town/category, mix it up.
  - Vary how (and whether) the Nina's Cleaning Service proof point comes up. Don't use the identical sentence every time; sometimes state it differently, sometimes skip it entirely if the message is strong without it. It's a proof point to reach for when useful, not a mandatory sentence.
  - Vary how Towpath Creative gets introduced. Not the identical "I run Towpath Creative, a small web shop in Frenchtown" every time, say it differently or fold it into the sign-off instead.
  - Vary the call-to-action phrasing. Not always "Do you have 5 to 10 minutes this week for a quick call?", there are a dozen ways to ask for a short call.
  - Lean harder into whatever's actually specific about this one business (the exact detail in the observation, something about the town, the category) as the backbone of the message, so it reads like it was written for them today, not pulled from a slot-filled template.

## What counts as a lead (two categories)
1. Local storefront/service businesses (retail, personal care, home services, independent professional services) with NO website at all, a broken/dead site, an outdated builder site (Wix/GoDaddy default template, clearly unmaintained), or a brand-new site with zero search presence.
2. Restaurants, cafes, bakeries, or other food-service businesses that either have no website, OR have a real website but NO way to order food online (no "order online" button/page, no DoorDash/Grubhub/UberEats/Toast/Square ordering link, no delivery mention anywhere on the homepage or menu page).

Exclude: chains/franchises with more than 3 locations, B2B companies, real estate agents, insurance agents, financial advisors, law firms, medical/dental practices, and any institutional or government entity. Stay local, independent, consumer-facing.

## Target towns (rotate, don't just hammer one)
Frenchtown NJ, New Hope PA, Lambertville NJ, Doylestown PA, Princeton NJ, Flemington NJ, Raritan NJ, Bucks/Hunterdon County generally.

## Primary research tool: the local Google Maps scraper (added 2026-09-17)
There is now a real, self-hosted tool for this instead of relying purely on WebSearch guessing: [gosom/google-maps-scraper](https://github.com/gosom/google-maps-scraper), built from source at `~/google-maps-scraper/google-maps-scraper` (Docker wasn't viable on this Intel Mac running macOS 12, so it's a native binary instead — see [[project_towpath_leadgen_agent_team]] for why).

This pulls structured data directly from Google Maps listings, which is what Google Business Profiles actually show, not a third-party B2B dataset guess. It directly reports a `website` field (empty = no site, on Google's own record) and an `order_online` field (empty = no online ordering link on the listing) per business, plus phone, category, address, and, with the `-email` flag, real emails extracted by crawling the business's own website (not guessed).

**How to run it:**
1. Write one query per line to a queries file, shaped like `<category> in <town>`, e.g. `restaurants in New Hope PA`, `hair salons in New Hope PA`, `hardware stores in New Hope PA`. Use several category queries per town rather than one broad query, for better coverage.
2. Run: `~/google-maps-scraper/google-maps-scraper -input <queries file> -results <output>.csv -email -depth 3 -exit-on-inactivity 2m`
3. Read the resulting CSV. Key columns: `title`, `category`, `address`, `phone`, `website`, `order_online`, `reservations`, `menu`, `emails`, `status` (open/closed), `review_count`, `review_rating`.

**How to read the output into a candidate/lead — confirmed against a real test run (Frenchtown NJ, 2026-09-17), two important corrections:**
- `website` empty → strong `none` candidate. (Confirmed real in testing: Napoli Pizza & Italian Restaurant had a genuinely empty website field.)
- `website` present but `status` shows permanently closed, or the field looks like a dead/parked domain → still needs an actual look (see verification below) before calling it `broken`.
- **`order_online` and `reservations` often contain the literal text `null` (not a blank cell) when absent — treat the string `"null"` the same as empty.** This is a CSV-writer quirk, not a value.
- **`order_online` being empty/null is a weak signal, not proof of "no ordering."** It only reflects whether the business wired up Google's own order-button integration on their Business Profile. In testing, every single restaurant in a real town had it empty, including ones with fully working sites and clear delivery presence elsewhere (DoorDash, Grubhub, Toast, their own site). Never mark a restaurant `no_ordering` from this field alone. It's a reason to look closer (check the actual website and search for delivery-platform listings), not a conclusion.
- **`emails` column needs filtering before it can be trusted — confirmed in testing it mixes real business emails with junk.** Real examples found: `chezdanielfrenchtown@gmail.com`, `artiesbar@gmail.com` (both clearly tied to the business name). Junk found in the same run: `hi@mystore.com` (a generic Square site-builder template placeholder, not this business's inbox), multiple `@sentry.io`/`@sentry-next.wixpress.com`/`@sentry.wixpress.com` addresses (error-tracking/telemetry addresses picked up from page source, not contact emails). **Rule: drop anything containing "sentry", "wixpress", "mystore.com", "noreply", "no-reply", "example.com", or any address that isn't plausibly tied to the specific business (matches the business name, is on the business's own domain, or is a personal-looking address with clear business context).** When in doubt, leave the email blank rather than pass along something that might not actually reach the business.
- Still apply the exclusion list below to every row (chains, B2B, real estate, insurance, financial, legal, medical/dental) before treating anything as a candidate — the scraper doesn't know Towpath's exclusion rules, it just returns what's on Google Maps.

This mostly replaces open-ended WebSearch guessing for the "does this business have a website" question, and gives a real (filtered) email most of the time instead of leaving it blank. WebSearch/WebFetch verification (below) still matters for: confirming a non-empty `website` URL is actually broken/outdated (vs. just fine and not a lead), a light double-check on `website`-empty results in case Google's own listing is simply stale, and — importantly — actually confirming online-ordering status for restaurants rather than trusting `order_online` alone.

## Verification rules — never skip, never fabricate
- A candidate isn't "verified" until you've actually looked, even one the scraper flagged. Don't guess or extrapolate from a business's category or size.
- For a candidate with a `website` URL from the scraper (broken/outdated/no_ordering cases): try to fetch it directly to confirm the actual condition. If WebFetch is blocked by network egress restrictions in your environment (EGRESS_BLOCKED errors happen, don't burn turns retrying the same domain), fall back to targeted searches combining the domain or business name with "order online", "menu delivery", "doordash", "grubhub" etc. to infer the same signals from search snippets and third-party delivery-platform pages. This fallback is a normal, expected method, not a last resort to apologize for.
- For a candidate with no `website` from the scraper: one confirming WebSearch for "<business name> <town> website" is still worth doing, since Google Maps listings can lag for very new businesses. If a real owned domain turns up that the scraper missed, that's disqualifying, not a lead.
- **Known trap, confirmed twice with a different data source**: don't trust any third-party dataset's "has no website" field as ground truth without checking yourself. Tested Vibe Prospecting/Explorium (unrelated to the Maps scraper above) against both general local businesses and restaurants — it flagged real, live businesses (Triumph Brewing Company, Cafe Blue Moose) as having no website when both clearly do. The Maps scraper is Google's own listing data, which is far more reliable, but "far more reliable" still isn't "never wrong" — the confirming step above exists for that reason.
- Only use a contact EMAIL from the scraper's `emails` column (extracted from the business's own site), or one you see actually displayed somewhere yourself. Never construct or guess one (no inventing info@ + their domain). Leave it blank if none is found — that's expected and fine for most of these.
- Note the specific evidence for every decision (what the scraper showed, what you additionally confirmed or didn't) — it goes in the lead's `observation` field and is what makes the lead credible to Kendall when he reviews it.

## The Lead Queue (Artifact database)
URL: (your Lead Queue artifact link)
Collection: `leads`

Document shape:
```
{
  "name": "<business name>",
  "town": "<town>",
  "category": "<e.g. restaurant, hardware store, hair salon>",
  "website_status": "<one of: none, broken, outdated, new_no_seo, no_ordering>",
  "phone": "<if found, else empty string>",
  "email": "<if found displayed somewhere, else empty string, never invent>",
  "observation": "<the specific evidence from verification>",
  "verified": true or false,
  "draft_subject": "<email subject line, under 60 chars>",
  "draft_message": "<email body>",
  "status": "new | contacted | interested | declined | won",
  "source": "manual | routine | team",
  "created_at": "<ISO timestamp>",
  "updated_at": "<ISO timestamp>"
}
```

Use `doc_id` as a lowercase hyphenated slug like `name-town-yyyymmdd`.

Use `source: "team"` for leads this agent team adds (as opposed to `manual` for Kendall's own entries or `routine` for the automated weekday cloud routine).

### Pipeline stage model
The dashboard derives a visible stage from these fields, it does not store a separate stage field:
- `status === "declined"` → **Declined**
- `status === "won"` → **Won**
- `status === "interested"` → **Interested**
- `status === "contacted"` → **Outreach**
- `draft_message` non-empty (and status is "new") → **Drafted**
- `verified === true` (and no draft yet) → **Verified**
- otherwise → **Research**

So: a fully worked lead from this team should land with `status: "new"`, `verified: true`, and both `draft_subject`/`draft_message` populated — it'll show up directly in the **Drafted** column, ready for Kendall to review and send from the Emails tab.

### Dedupe
Before adding anything, query the `leads` collection filtered by `town` and skip anything that already matches loosely on name (ignore punctuation/case/LLC suffixes).

## Relationship to the automated cloud routine
There is also a separate automated weekday routine ("Towpath Creative — Local Lead Prospecting") that runs this same playbook on its own every weekday morning against one rotating town. This agent team is for **on-demand** work: Kendall asking for a specific town right now, wanting more leads than the routine's daily cap, or wanting a second look at a specific business. Both write to the same Lead Queue, so always dedupe against it regardless of source.

## Never
- Never send an email or message on Kendall's behalf. This team only ever writes to the review queue. Kendall reviews and sends manually.
- Never fabricate a business, a fact about a business, or a contact detail.
