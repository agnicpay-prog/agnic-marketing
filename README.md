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

Just talk to Claude naturally — the CMO skill is model-invoked and will pick itself up on marketing questions. To force a specific skill, call it directly:

```
/agnic-marketing:cmo what should we focus on this week?
/agnic-marketing:ai-visibility draft the audit pitch for a chiropractor
/agnic-marketing:copywriter /email write a cold outreach email to a dental clinic
/agnic-marketing:community /linkedin write a client-win post about our first case study
/agnic-marketing:builder-relations /partner-program draft the partner one-pager
/agnic-marketing:growth /dashboard fill in this week's numbers
```

The `cmo` skill also understands its own slash-style commands referenced in its frontmatter (`/gtm`, `/campaign`, `/launch`, `/pricing`, `/icp`, `/positioning`, `/weekly-plan`, `/monthly-plan`, etc.) — just include the command word in your message, e.g. `/agnic-marketing:cmo /weekly-plan`.

If you're not sure which specialist owns something, ask `cmo` — it routes internally and will pull in `growth`/`copywriter`/`community` etc. as needed rather than making you guess.

## Keep it up to date

The marketplace auto-refreshes in the background, but to force it:

```
/plugin marketplace update
```

If someone bumps `version` in `.claude-plugin/plugin.json` after editing a skill, you'll get prompted to update; otherwise every new commit to `main` counts as a new version automatically (we haven't pinned a `version`, so this is the default behavior here — just pull the latest and reload).

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
