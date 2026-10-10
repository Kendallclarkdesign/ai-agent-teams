# AI agent teams

Thirteen custom Claude Code subagents in three teams, designed so each step can catch the mistakes of the one before it. These are the real prompt files with private details removed. Part of Kendall Clark's portfolio.

Case study: https://kendallclark.design/case-study-agent-teams

| Team | Agents | Job |
| --- | --- | --- |
| `leadgen/` | project manager, local lead researcher, lead verifier, outreach copywriter, lead queue data manager | Find local businesses that need a website, verify each one, and draft outreach |
| `marketing/` | content director, graphic designer, video producer, content writer, brand strategist | Produce graphics, video, copy, and brand content, wired to Canva, Figma, and video tools |
| `reports/` | report manager, data analyst, report writer | Build a client progress report where every number traces to a checked source |

## Design rules

- **Managers delegate and never do the work.** Judgment and production stay separate.
- **Permissions shape behavior.** The outreach writer has no research tools, so it can only use facts the verifier confirmed. Only the data manager can write to the lead database, and it checks for duplicates first.
- **Finding is not concluding.** The researcher surfaces candidates and never decides that a business has no website. The verifier makes that call.
- **One reference file per team** (`reference.md`) holds facts, voice rules, and schemas, so a fix happens in one place.
- **Every number needs a source.** If a source is missing, the agent says so instead of guessing.

## How to use

Copy a team folder into `~/.claude/agents/` (or a project's `.claude/agents/`), and put its `reference.md` where the agent files say to read it. Replace the placeholder contact details with your own.

## Status

The lead-generation team has been run live on two towns. The marketing and report teams are built and documented but have not yet had a full live run as a team.
