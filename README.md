# agnic-marketing

Agnic's AI marketing org, packaged as a Claude Code / Claude Cowork plugin. One CMO orchestrator skill plus seven specialists:

- `cmo` — GTM strategy, prioritization, campaign/launch planning, weekly/monthly reviews. Orchestrates everything below.
- `growth` — funnels, growth loops, referrals, experiments, dashboards.
- `copywriter` — landing pages, emails, social copy, sales collateral.
- `product-marketing` — positioning, ICP, pricing, case studies, launches.
- `community` — LinkedIn, X, Reddit, Hacker News, Product Hunt, founder brand.
- `ai-visibility` — the AI Visibility Audit/Optimization offer and GEO technique (primary revenue motion).
- `builder-relations` — the AI Builder Partner Program.
- `events` — conferences, sponsorships, workshops, webinars.

Plus reusable `campaigns/`, `templates/`, and `dashboards/` reference docs the skills point to.

This repo is both the **plugin** (`.claude-plugin/plugin.json`) and its own **marketplace** (`.claude-plugin/marketplace.json`), so it can be installed directly from git with no separate marketplace repo.

## Try it locally (no install)

```bash
claude --plugin-dir /path/to/agnic-marketing
```

Then run any skill, e.g. `/agnic-marketing:cmo`.

## Install for yourself

```bash
# from inside a Claude Code session
/plugin marketplace add /path/to/agnic-marketing
/plugin install agnic-marketing@agnic-tools
```

Once pushed to a git remote (GitHub, GitLab, etc.), swap the local path for the repo:

```bash
/plugin marketplace add <owner>/<repo>          # GitHub shorthand
/plugin marketplace add https://gitlab.com/<org>/<repo>.git   # other git hosts
/plugin install agnic-marketing@agnic-tools
```

Or non-interactively:

```bash
claude plugin marketplace add <owner>/<repo>
claude plugin install agnic-marketing@agnic-tools
```

## Share with the team

1. Push this repo to your team's git host (GitHub/GitLab/etc.) — private repo is fine, Claude Code supports private marketplaces.
2. Everyone on the team runs the two `/plugin marketplace add` / `/plugin install` commands above, once.
3. To make the marketplace load automatically for anyone who trusts the project folder, add this to a shared `.claude/settings.json` in your main codebase:

```json
{
  "extraKnownMarketplaces": {
    "agnic-tools": {
      "source": { "source": "github", "repo": "<owner>/<repo>" }
    }
  },
  "enabledPlugins": {
    "agnic-marketing@agnic-tools": true
  }
}
```

4. To update after editing skills: bump `version` in `.claude-plugin/plugin.json`, push, then teammates run `/plugin marketplace update` (or it auto-refreshes in the background). If you don't set `version`, every new commit counts as a new version automatically.

## Use it in Claude Cowork / claude.ai

Cowork, the claude.ai Chat tab, and Claude Desktop all install plugins from the same kind of git-based marketplace:

1. In Cowork/claude.ai, go to the Plugins page and add this repo's URL as a marketplace (or add it under your organization's marketplaces so the whole team can install from it).
2. Install `agnic-marketing` from that marketplace.
3. For a one-off/manual install instead of git, zip this directory and upload it directly on the Plugins page (`.zip`, under 50MB).

## Validate before pushing

```bash
claude plugin validate .
```

## Editing

- Add a new specialist: create `skills/<name>/SKILL.md` with frontmatter (`name`, `description`), it's picked up automatically — no manifest changes needed.
- Add a new campaign/template: drop a `.md` file in `campaigns/` or `templates/` and link to it from the relevant skill.
- Keep cross-references to other skills as plain skill names (e.g. "the `growth` skill") rather than file paths — skills are invoked, not read as files. Cross-references to `campaigns/`, `templates/`, `dashboards/` should stay as relative markdown links since those are reference docs, not skills.
