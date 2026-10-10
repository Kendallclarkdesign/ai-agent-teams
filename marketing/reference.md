# Towpath Creative — Marketing Content Team Reference

Read this before delegating or producing any graphic, video, copy, or brand content. It has the standing voice rules, where to verify brand facts, and which tool to reach for.

## Who this team serves
This team works two kinds of jobs:
1. **Towpath Creative's own brand** — working folder `~/towpath-creative/` (the stale draft `~/towpath-creative-OLD-DRAFT-do-not-use` is NOT current, never pull facts from it), live at towpathcreative.com.

If it's not obvious which, ask before producing anything.

## Standing content rules (never break these)
- **No em dashes, anywhere, ever.** Not in copy, not in image text, not in video scripts.
- **No generic AI-slop language.** Banned: seamless, leverage, robust, cutting-edge, game-changer, unlock, delve, elevate, unleash, next-gen, "in today's fast-paced world," and similar filler. Plain, specific, active-voice language only.
- **If Kendall gives a reference screenshot or existing asset, reproduce every visible detail** — layout, spacing, colors, type — don't extrapolate a "close enough" version from source code or memory alone.
- **Never create or publish anything live** (a live Shopify product, a sent email campaign, a published social post, a live site deploy) without Kendall's explicit sign-off. Draft it, show it, wait for approval.

## Verify brand facts live — don't trust memory or old files
Client brand details (colors, fonts, voice, current live copy) can go stale fast. Before producing anything for a client:
- Check the client's actual local project folder for current source files.
- Check the live URL if one exists — it's the real source of truth, not necessarily what's in the local folder.
- Never invent a brand fact (a color hex, a tagline, a phone number) that you haven't actually confirmed from the client's real files or live site.

## Known project folders (verify these still exist and are current — don't assume)
- Towpath Creative (own brand): `~/towpath-creative/`
- HLH Pipeline: `~/hlh-pipeline/` + `~/Documents/HL01_Remotion/` — lavender/rose brand, Remotion-based video pipeline
- 21 Connect Digital client work: `~/Documents/21 Connect Digital/`

If a folder listed here doesn't actually exist when you check, say so — don't guess a substitute path.

## Tool map — what to reach for
| Job | Tool |
|---|---|
| Branded static graphics (social posts, flyers, ads, product graphics) from a brand kit or template | **Canva MCP** (`generate-design`, `create-design-from-brand-template`, brand kit tools) |
| Pixel-exact reproduction of a design reference, or web/product UI assets | **Figma MCP** (`get_design_context`, `get_screenshot`, `use_figma`) |
| AI-generated product marketing video, UGC-style ads, avatars, short-form video | **higgsfield MCP** — `marketing_studio_v2_*` (purpose-built for product marketing content) and `shorts_studio_*` for short-form |
| Script-to-finished-video without the higgsfield toolchain | **invideo MCP** (`generate-video-from-script`) |
| Code-based, fully custom video (e.g. HLH Pipeline style) | **Remotion** — see the `remotion-best-practices` skill and `~/hlh-pipeline/` |
| Email graphics or campaign visuals | **Mailchimp MCP** (`generate_image`, `edit_image`) |
| Charts or data visuals inside any content | **`dataviz` skill** — load it before building any chart |
| Palette / type-pairing / component style decisions | **`ui-ux-pro-max` skill** |
| Keyword or competitive research to inform copy/business content | **Ahrefs or Semrush MCP** (already connected) |

## Cost awareness
higgsfield video/image generation and Canva both consume credits. Before a large batch (multiple videos, a batch image run), check balance/cost tools first and flag the estimated cost to Kendall rather than just running it.
