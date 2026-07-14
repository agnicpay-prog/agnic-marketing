# agnic-marketing

Agnic's AI marketing org, packaged as a Claude Code / Claude Cowork plugin. One CMO orchestrator skill plus seven specialists, callable from any Claude Code session or Cowork chat once installed.

Repo: **https://github.com/agnicpay-prog/agnic-marketing** (private)

---

## What's inside

| Skill | Invoke as | Use for |
|---|---|---|
| `cmo` | `/agnic-marketing:cmo` | GTM strategy, prioritization, campaign/launch planning, weekly & monthly reviews. The orchestrator — start here if unsure which skill you need. |
| `growth` | `/agnic-marketing:growth` | Funnels, growth loops, referrals, experiment design, the weekly dashboard. |
| `copywriter` | `/agnic-marketing:copywriter` | Landing pages, emails, social copy, sales collateral — actual copy, not descriptions of copy. |
| `product-marketing` | `/agnic-marketing:product-marketing` | Positioning, ICP definitions, pricing, case studies, launches. |
| `community` | `/agnic-marketing:community` | LinkedIn, X, Reddit, Hacker News, Product Hunt, founder brand strategy. |
| `ai-visibility` | `/agnic-marketing:ai-visibility` | The AI Visibility Audit → Optimization offer and GEO technique — Agnic's primary revenue motion. |
| `builder-relations` | `/agnic-marketing:builder-relations` | The AI Builder Partner Program — recruiting and enabling resellers. |
| `events` | `/agnic-marketing:events` | Conferences, sponsorships, workshops, webinars. |

Every skill shares the same voice (Jason Lemkin GTM ruthlessness + Dave Gerhardt founder-led marketing + Harry Dry copywriting) and ends its answers with a `Priority / Why / Deliverables / KPIs / Next 3 actions / Expected outcome / Risks` block. See `skills/cmo/SKILL.md` for the full operating manual.

Supporting reference docs the skills point to:
- `campaigns/` — worked campaign briefs (AI Visibility launch, Builder Partner recruiting, workshops, conference evaluation).
- `templates/` — fill-in-the-blank templates (LinkedIn, email, landing page, workshop agenda, case study).
- `dashboards/weekly-growth.md` — the weekly numbers template, owned by `growth`.

This repo is both the **plugin** (`.claude-plugin/plugin.json`) and its own **marketplace** (`.claude-plugin/marketplace.json`) — no separate marketplace repo needed.

---

## Quickstart: install it (Claude Code, team members)

One-time setup, run inside any Claude Code session:

```
/plugin marketplace add agnicpay-prog/agnic-marketing
/plugin install agnic-marketing@agnic-tools
```

Or non-interactively from your shell:

```bash
claude plugin marketplace add agnicpay-prog/agnic-marketing
claude plugin install agnic-marketing@agnic-tools
```

You'll need GitHub access to `agnicpay-prog/agnic-marketing` (it's private) — ask whoever manages the org to add you as a collaborator, and make sure `gh auth status` or your git credential helper is authenticated before installing.

Once installed, restart or run `/reload-plugins`, then confirm it loaded:

```
/help
```

You should see the 8 skills listed under the `agnic-marketing` namespace.

## How to use it

Just talk to Claude naturally — the CMO skill is model-invoked and will pick itself up on marketing questions. You can also call any skill directly:

```
/agnic-marketing:cmo what should we focus on this week?
/agnic-marketing:ai-visibility draft the audit pitch for a chiropractor
/agnic-marketing:copywriter /email write a cold outreach email to a dental clinic
/agnic-marketing:community /linkedin write a client-win post about our first case study
/agnic-marketing:builder-relations /partner-program draft the partner one-pager
/agnic-marketing:growth /dashboard fill in this week's numbers
```

The `cmo` skill also understands its own slash-style commands referenced in its frontmatter (`/gtm`, `/campaign`, `/launch`, `/pricing`, `/icp`, `/positioning`, `/weekly-plan`, `/monthly-plan`, etc.) — just include the command word in your message, e.g. `/agnic-marketing:cmo /weekly-plan`.

### The best way to use it: start with `cmo` for anything real

For a one-off asset you already know you want ("write a LinkedIn post about our first case study"), calling the specialist directly is fine and fast.

But for anything that's actually a **decision** — a campaign, a launch, "should we do X or Y," a weekly/monthly plan — start with `/agnic-marketing:cmo` and let it run its full planning workflow instead of jumping straight to a specialist. `cmo` will:

1. **Ask you sharp clarifying questions first** instead of guessing — what's actually been tried, what the real constraint is, what winning looks like.
2. **Draft 2-3 real options**, not just one.
3. **Delegate the actual drafting** to the right specialist(s) (`growth`, `copywriter`, `ai-visibility`, etc.) with a scoped brief.
4. **Review and push back on their drafts** — send weak work back for a revision before it reaches you.
5. **Compare the finished options side by side** and recommend one, with the reasoning shown, not just the answer.
6. **Hand you one synthesized plan** — a campaign/launch brief with KPIs, next actions, and an owner — closing every answer with a `Priority / Why / Deliverables / KPIs / Next 3 actions / Expected outcome / Risks` block.

If you're not sure which specialist owns something, that's another reason to start at `cmo` — it routes internally rather than making you guess.

### Sample: a real planning conversation

```
You:  /agnic-marketing:cmo we want more AI Visibility clients this month —
      should we run a workshop or just do more cold outreach?

cmo:  [Step 1 — Clarify] Before I plan this:
      1. What's happened with cold outreach so far — how many sent, how many booked?
      2. Do we have access to a room of real clinic owners (association, network),
         or would we be building that list from scratch?
      3. Is the goal booked audits this month specifically, or a repeatable channel
         long-term? Those lead to different plans.

You:  Sent ~15 cold emails, booked 2 audits. We know the head of the regional
      dental association — she could get us 15-20 attendees. Goal is booked
      audits this month.

cmo:  [Step 2 — Frame] Objective: maximize booked Audits in the next 30 days.
      Bottleneck: outbound alone converts slowly (2/15 ≈ 13%) and scaling it
      further just repeats that rate. Tier 1.

      [Step 3 — Options]
      A. Scale cold outreach 3x         — cheap, proven-ish, but low per-lead conversion
      B. Run the workshop               — higher trust/conversion, one-time setup cost
      C. Hybrid: workshop + outreach to non-attendees after

      [Step 4 — Delegating] Pulling in `events` to draft the workshop agenda
      and `growth` to model expected conversion for each option...

      [Step 5 — Review] events' first agenda draft buried the live-audit demo
      at minute 40 — sent back: "lead with the demo in the first 10 minutes,
      that's the only reason someone shows up." Revised draft looks right.

      [Step 6 — Compare]
      OPTION A: 45 more cold emails  → ~6 booked audits, founder time only
      OPTION B: workshop, 15-20 attendees → ~5-6 booked audits same-day, plus a
        reusable format and case-study content
      OPTION C: workshop + follow-up outreach to no-shows → best of both,
        one extra week of founder time

      RECOMMENDATION: C. The workshop gets same-day conversions AND the list of
      non-converters becomes a warmer outreach batch than cold — outbound gets
      better, not just repeated.

      [Step 7 — Plan]
      CAMPAIGN: "Is Your Practice Invisible to AI?" Workshop + Follow-up
      Priority tier: 1
      Objective: 8+ booked Audits in 30 days
      ... (full brief, using campaigns/workshop.md as the base)

      Priority: Tier 1 — this is this month's #1 revenue lever
      Why: Highest realistic conversion given what outbound alone has shown so far
      Deliverables: workshop agenda, invite list ask (association), live-audit
        script, next-day follow-up email
      KPIs: attendees (target 15-20), audits booked within 48h (target 8+)
      Next 3 actions: (1) confirm date with association contact — you, this week,
        (2) finalize live-audit script — events skill, 2 days, (3) draft follow-up
        email — copywriter skill, in parallel
      Expected outcome: 8+ booked audits, a reusable workshop format, and a
        warmer outreach list for anyone who doesn't convert on the spot
      Risks: attendee no-shows if the association ask falls through — have a
        backup invite list ready
```

That's the shape to expect for any real planning question — not just a one-shot answer, but a visible process you can interrupt or redirect at any step (e.g. "actually we can't do a workshop this month" sends it back to re-compare A and C only).

## Keep it up to date

The marketplace auto-refreshes in the background, but to force it:

```
/plugin marketplace update
```

`.claude-plugin/plugin.json` pins an explicit `version` (currently `1.1.0`) — you only get an update when that number changes, so if a skill edit doesn't seem to be taking effect, check that whoever made the change bumped it and that you've run `/plugin marketplace update`.

## Make it load automatically for the whole team

Add this to `.claude/settings.json` in Agnic's main codebase (not this repo) so anyone who opens that project gets prompted to install automatically:

```json
{
  "extraKnownMarketplaces": {
    "agnic-tools": {
      "source": { "source": "github", "repo": "agnicpay-prog/agnic-marketing" }
    }
  },
  "enabledPlugins": {
    "agnic-marketing@agnic-tools": true
  }
}
```

## Use it in Claude Cowork / claude.ai / Claude Desktop

Cowork, the claude.ai Chat tab, and Claude Desktop install plugins from the same kind of git-based marketplace as Claude Code:

1. Go to the **Plugins** page in Cowork/claude.ai.
2. Add `agnicpay-prog/agnic-marketing` as a marketplace (or ask your org admin to add it under the organization's marketplaces so everyone on the team can install from it without doing this step individually).
3. Install `agnic-marketing` from that marketplace.
4. Alternative for a one-off, no-git-access install: clone this repo, zip the `agnic-marketing/` directory, and upload it directly on the Plugins page (`.zip`, under 50MB). This won't auto-update — prefer the marketplace route for anything ongoing.

## Try changes locally before pushing (for anyone editing skills)

```bash
git clone https://github.com/agnicpay-prog/agnic-marketing.git
cd agnic-marketing
# edit skills/*/SKILL.md, campaigns/*.md, templates/*.md, etc.
claude plugin validate .                 # checks marketplace.json + plugin.json + skill frontmatter
claude --plugin-dir .                    # loads your local edits in a live session, no install needed
```

Inside that session, run `/reload-plugins` after further edits instead of restarting.

When ready, push to `main` (or open a PR if your team wants review) and optionally bump `version` in `.claude-plugin/plugin.json` to force teammates to pick up the update on their next `/plugin marketplace update`.

### Adding new content

- **New specialist skill**: create `skills/<name>/SKILL.md` with `name`/`description` frontmatter — it's picked up automatically, no manifest edits needed.
- **New campaign/template**: drop a `.md` file into `campaigns/` or `templates/` and link to it from the relevant skill.
- **Cross-referencing**: reference other skills by name in prose (e.g. "the `growth` skill") rather than by file path — skills are invoked (`/agnic-marketing:growth`), not read as files. Links to `campaigns/`, `templates/`, `dashboards/` should stay as relative markdown links (e.g. `[../../templates/email.md](../../templates/email.md)` from inside `skills/`) since those are reference docs Claude reads directly, not invokable skills.

## Troubleshooting

- **`plugin-not-found` or marketplace won't add**: confirm you have GitHub access to `agnicpay-prog/agnic-marketing` and that `gh auth status` (or your git credential helper) is authenticated.
- **Skills not showing up after install**: run `/reload-plugins`, or fully restart Claude Code.
- **Validation errors before pushing**: run `claude plugin validate .` from the repo root — it checks `marketplace.json`, `plugin.json`, and every skill's YAML frontmatter in one pass.
- **Private-repo auto-updates failing silently**: background marketplace refreshes use a stricter auth path than manual commands. Run `/plugin marketplace update` manually if it seems stale, or see Claude Code's plugin-marketplaces docs for the private-repo auth notes.
