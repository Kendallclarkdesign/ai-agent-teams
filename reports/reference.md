# Towpath Creative — Client Report Team Reference

Read this before drafting, coordinating, or finishing any client progress report. It has the standing accuracy rules, the report structure to use as the default going forward, the data sources to pull from, and how to finish and deliver the report.

## Who this team serves

Any Towpath Creative client with a live site and some combination of analytics, a booking/contact form, and a Google Business Profile. Confirmed so far: Nina's Cleaning Service (`~/Documents/21 Connect Digital/ninas-cleaning-service/`, yes, the folder path says "21 Connect Digital," but the real agency of record is Towpath Creative, never put "21 Connect Digital" on a client-facing report). Other client folders may apply per `~/.claude/towpath-marketing-reference.md`'s known project list, verify the folder actually exists and has the right data sources before assuming this template fits.

## The one rule that overrides everything else

**Every number in a client report must trace back to a real, checked source.** Git commit history, a live analytics pull, a live form-submission count, a curl check against the production URL, or numbers the client themselves supplied (like a GBP screenshot). Never estimate, never reuse a stale number from a prior report as if it's current, never fabricate a metric to make the report read better. If a number isn't available, say so plainly in the report rather than guessing or omitting silently. This rule is not negotiable, even when Kendall asks for a more impressive or high-value framing, elevate the language and the framing, never the underlying numbers.

## Report structure (default going forward)

Built and refined on the Nina's Cleaning Service August/September 2026 report. Use this structure for every client report unless Kendall says otherwise:

1. **Masthead** — agency name (verify which one is correct for this client, don't assume from a folder path), client name, reporting period.
2. **Intro** — one to two short paragraphs: what period this covers, and briefly what shipped. If Kendall wants a top-tier-agency value framing, describe the methodology and discipline as equivalent to a $X,XXX/month agency engagement. Never state that dollar figure as the client's actual bill.
3. **At-a-glance figures** — 3 to 4 headline numbers in the figures grid, each independently verifiable.
4. **Itemized work sections** — one per real, shipped change or finding. Each gets a plain-language headline, a short explanation of what changed and why it mattered (tie back to an audit finding or a real problem when one exists), and a benefit callout in the client's own terms, not agency jargon.
5. **Traffic section** — show every traffic source/channel, not just the top few. Compute one or two honest standout metrics from the real data (new-visitor share, a page's pull relative to the homepage) rather than a generic "visits went up" line.
6. **Google Business Profile section**, when data is available — no reliable automated connector exists as of this writing (checked: not present in Claude's MCP roster, not even behind an auth-required listing). Get the numbers from Kendall or the client as a screenshot or pasted text from business.google.com's Performance tab, and say plainly in the report that this came from a manual pull, not an automated feed. Report the real trend even when it's not flattering (calls dropping while website clicks rise, for example), and cross-reference against the site's own traffic numbers where there's a real, defensible connection.
7. **"What the data says to build next"** — the forward-looking section. Every item here is a real, underdeveloped area the data actually shows (a page nobody's visiting yet, a channel lagging another, a stat that dropped, a visible gap like invisible pricing or an unconfirmed trust claim), each with a concrete next action. This is not a generic to-do list. Admin-only items (a directory listing rejected pending paperwork, for example) belong inline in the relevant work section instead, not in this growth-focused list.
8. **Close** — short, no filler, thank-you plus a one-line nod to next period. Sign with the correct agency name only. No "Prepared on [date]" line (Kendall's preference, confirmed 2026-09-22).

## House copy rules (apply to every report, no exceptions)

Same as the rest of Towpath's marketing content rules in `~/.claude/towpath-marketing-reference.md`: no em dashes, no AI-slop filler (seamless, leverage, robust, cutting-edge, game-changer, unlock, delve, elevate, unleash, next-gen, "in today's fast-paced world"), active voice, short sentences, one idea per sentence where possible. Keep paragraphs short enough to scan. Split a dense paragraph into two rather than cramming multiple ideas into one.

## Data sources and how to pull them

| Data | Source | How |
|---|---|---|
| What actually shipped on the site | `git log --oneline --date=short --pretty=format:"%ad  %s" --all` in the client's project folder | Only report changes with a real commit (or a verified live check) behind them. Don't infer work from file modification times alone, they can be misleading (checkouts, re-fetches). |
| Website traffic | The client's GA4 property, via a `ga4-report.py`-style script if one exists in the project (check `scripts/`), else via the `seo-google` skill or Search Console | Pull `overview`, `top_pages`, and `acquisition` for the exact reporting period. Needs a numeric GA4 Property ID and the service account added as a Viewer on the property (Admin > Property Access Management) before it authenticates. A 403 means that step hasn't been done yet, tell Kendall rather than working around it. |
| Booking/contact form submissions | Netlify Forms, via the Netlify MCP reader (`netlify-project-services-reader`: `get-projects` to find the site ID, then `get-forms-for-project`) if the site is on Netlify | This only gives a lifetime `submission_count` and `last_submission_at`, not a per-submission date breakdown. If you need this period's number, subtract the count already reported in the prior report and show the subtraction. |
| Live site verification | `curl -s -o /dev/null -w "%{http_code} -> %{redirect_url}"` against the production URL | Never claim a fix is live without checking it against production, not just the local file. |
| Google Business Profile | No automated connector exists | Ask Kendall or the client for a screenshot or pasted text from business.google.com's Performance tab. Never reuse a prior period's numbers, never estimate. |
| Prior report context | The client's `crew/` folder (or wherever past reports live) | Read the last report to get the period start, what was already reported (avoid double counting), and open items still outstanding. |

## Finishing and delivering the report

1. Draft as a single self-contained HTML file (inline `<style>`, no external assets) so it opens directly and renders to PDF cleanly. Reuse the client's own established report design if one exists, check `crew/` for a prior report to match colors and typography. Don't impose a different client's brand palette onto this one.
2. Include a `@media print` block in the CSS (color-adjust settings, safe page-break rules) so the PDF looks intentional, not like a browser page squeezed onto paper.
3. Render to PDF with headless Chrome:
   ```
   "/Applications/Google Chrome.app/Contents/MacOS/Google Chrome" --headless=new --disable-gpu --no-pdf-header-footer --print-to-pdf="<output>.pdf" --print-to-pdf-no-header "file://<absolute path to html>"
   ```
   Confirm the output file is a reasonable size, a blank or broken render is usually near-zero bytes.
4. Deliver both the `.html` source (for future edits) and the `.pdf` (the actual client-facing deliverable) to Kendall, using `SendUserFile` so it reaches him even if he's not at this machine.
5. Never send anything to the client directly. Kendall reviews and sends it himself.
