# ai-txt — Claude Code skill

A Claude Code skill that generates and audits the files that control or inform AI systems about a website:

- **`robots.txt`** with AI-specific user-agents (GPTBot, ClaudeBot, Perplexity, Google-Extended, Applebot-Extended, …)
- **`ai.txt`** in [aitxt.ing](https://aitxt.ing) format (anti-hallucination context)
- **`llms.txt`** (curated docs map for LLMs)
- **`/.well-known/tdmrep.json`** (EU CDSM Article 4 TDM opt-out)
- Spawning-style `ai.txt` when explicitly requested

The skill knows the difference between the three unrelated specs that all share the name "ai.txt" and picks the right file for the user's actual goal.

## What it does

**Generate mode** — ask it for a file and it produces the right one:

> *"Block AI training on my site but keep ChatGPT and Perplexity search referrals."*
> → `robots.txt` with per-vendor user-agents, training blocked, search bots allowed.

> *"ChatGPT keeps inventing features that don't exist in my app."*
> → aitxt.ing-format `/ai.txt` with a "What we do NOT offer" section that explicitly lists the hallucinations.

**Audit mode** — point it at an existing file and it reports:

- Deprecated tokens (`anthropic-ai`, `claude-web`)
- Missing modern AI user-agents (as of April 2026)
- The Applebot-Extended / Googlebot inheritance trap
- Training-vs-search policy mismatches

## Installation

### Option 1 — Add to a specific project (via `skills-lock.json`)

In the project root's `skills-lock.json`:

```json
{
  "version": 1,
  "skills": {
    "ai-txt": {
      "source": "firstpoint/ai-txt-skill",
      "sourceType": "github"
    }
  }
}
```

Next Claude Code session in that project will fetch the skill automatically.

### Option 2 — Global install (available in every project)

```bash
git clone https://github.com/firstpoint/ai-txt-skill.git ~/.claude/skills/ai-txt
```

Or, if you prefer the packaged `.skill` file:

```bash
unzip ai-txt.skill -d ~/.claude/skills/
```

### Option 3 — Per-project manual install

```bash
git clone https://github.com/firstpoint/ai-txt-skill.git /path/to/project/.claude/skills/ai-txt
```

## Usage

Once installed, just ask naturally in Claude Code:

- *"write me a robots.txt that blocks AI training"*
- *"add an ai.txt to this Next.js project so ChatGPT stops making up features"*
- *"check this robots.txt — is it up to date for 2026?"*
- *"generate an llms.txt for our docs site"*

The skill triggers on these phrasings and walks you through the right file for your goal.

## What's inside

```
ai-txt/
├── SKILL.md              # the skill itself (instructions for Claude)
├── LICENSE               # MIT
├── README.md             # this file
└── references/
    ├── user-agents.md    # current AI bot list (Apr 2026) + traps
    └── examples.md       # copy-paste templates for each file type
```

The `references/` files are loaded on demand — Claude reads them only when the skill needs concrete data (bot names, IP ranges, file templates) so the main prompt stays lean.

## Keeping it current

The AI user-agent list drifts: vendors add new bots, retire old ones, change policies. `references/user-agents.md` is dated (April 2026); PRs welcome when new agents appear.

## License

MIT — see [LICENSE](./LICENSE).

Copyright © 2026 FIRST POINT YAZILIM LİMİTED ŞİRKETİ
contact@firstpoint.com.tr · https://firstpoint.com.tr/

---

Built by **[First Point Yazılım](https://firstpoint.com.tr/)** — Ankara, Turkey.
