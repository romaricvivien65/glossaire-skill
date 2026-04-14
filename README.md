<p align="center">
  <img src="banner.png" alt="Glossary — Living Personal Vocabulary for Claude Code" width="100%">
</p>

# Glossary — Living Personal Vocabulary for Claude Code

A skill that turns Claude Code into your personal vocabulary coach. Capture words on the fly, organize them into categories, quiz yourself, and explore new domains — all from your terminal.

## The Problem

You encounter dozens of new terms every week — in meetings, code reviews, articles, client calls. Most of them vanish from memory within days. Bookmarks pile up. Notes get lost. And when the term comes up again, you're back to square one.

**Glossary** gives you a structured, searchable, quizzable knowledge base that grows with you — powered by your AI coding assistant.

## What It Does

```
New word encountered
    | Mode 1-2: Capture
Web search + definition + save to markdown
    | Mode 3: Review
Organize basket into categories
    | Mode 7: Browse
Search, stats, recent additions
    | Mode 8: Quiz
Flashcards, multiple choice, series
    | Mode 9: Explore
Discover vocabulary of entire domains
```

### 10 Modes

| Mode | Name | What it does |
|------|------|-------------|
| 0 | **Help** | Display usage guide |
| 1 | **Quick Capture** | Search + define + save to basket |
| 2 | **Full Capture** | Search + define + classify immediately |
| 3 | **Basket Review** | Organize unclassified words into categories |
| 4 | **Simple Definition** | Answer a definition question + offer to save |
| 5 | **Language Rules** | Grammar/spelling questions saved as rules |
| 6 | **Quotes** | Save quotes with verified attribution |
| 7 | **Browse** | Search, stats, browse by category or recent |
| 8 | **Quiz** | Flashcards, multiple choice, reverse quiz, series |
| 9 | **Exploration** | Discover key terms of any domain in bulk |

### Output: a `Glossary/` folder

```
Glossary/
├── INDEX.md              <- Dashboard: all words at a glance
├── README.md             <- User guide
├── _basket/              <- Words captured quickly, not yet classified
│   └── compliance.md
├── engineering/           <- Categories created on demand
│   ├── idempotent.md
│   └── webhook.md
├── marketing/
│   └── product_market_fit.md
├── language/              <- Grammar and spelling rules
│   └── affect_vs_effect.md
├── quotes/                <- Saved quotes with attribution
│   └── einstein_imagination_more_important.md
└── ...                    <- New categories emerge as needed
```

### Word file format

```markdown
---
word: Product-Market Fit
category: marketing
tags: [strategy, startup]
date: 2026-04-10
source: Article on First Round Review
---

# Product-Market Fit

## Definition
The degree to which a product satisfies strong market demand.
Coined by Marc Andreessen: "being in a good market with a product
that can satisfy that market."

## In Context
During the board meeting, the CEO said we hadn't reached PMF yet
because churn was still above 8% monthly.

## Personal Notes
Related to Sean Ellis test: "How would you feel if you could
no longer use this product?" — 40%+ "very disappointed" = PMF.
```

## Key Features

- **Web-powered definitions**: every word is researched online, never made up
- **Duplicate detection**: checks existing glossary before adding
- **Smart suggestions**: Mode 4 bridges daily work and vocabulary building
- **Attribution verification**: Mode 6 fact-checks quote authors
- **Tag system**: filter and quiz by thematic families across categories
- **Batch exploration**: Mode 9 lets you learn 10-15 terms of a new domain in one session
- **Spaced review**: flashcards, MCQ, reverse MCQ, and scored series

## Installation

### Claude Code

Copy the `skills/glossaire/` folder into your Claude Code skills directory:

```bash
# Personal skills (available in all projects)
cp -r skills/glossaire ~/.claude/skills/glossaire

# Or project-specific
cp -r skills/glossaire .claude/skills/glossaire
```

Then create your glossary root directory and invoke with `/glossary` in Claude Code.

### Other AI Coding Agents

The `SKILL.md` file is a standard skill format. It can be adapted for:
- **GitHub Copilot CLI** — place in your skills directory
- **Gemini CLI** — activate via `activate_skill`
- **Any LLM agent** — use the SKILL.md as a system prompt or reference document

## Use Cases

- **Developer onboarding** — Capture domain-specific jargon as you encounter it in code reviews and meetings
- **Career growth** — Build vocabulary in new areas (management, legal, finance) as your role evolves
- **Language learning** — Save grammar rules and vocabulary in a structured, quizzable format
- **Client communication** — Learn client industry terms before meetings, review with flashcards
- **Quote collection** — Build a curated, verified quote library organized by theme
- **Domain exploration** — Rapidly learn the vocabulary of a new field with Mode 9's guided discovery

## Design Principles

1. **Capture first, organize later.** The basket reduces friction. Don't force classification at capture time.
2. **Search, don't invent.** Every definition comes from a web search. The skill synthesizes, never fabricates.
3. **Grow organically.** Categories emerge from actual words, not from a predefined taxonomy.
4. **Learn by doing.** Quiz modes use your own words, not generic vocabulary lists.
5. **Respect the flow.** Mode 4 suggests saving without interrupting. A "no" is always accepted.

## License

MIT — see [LICENSE](LICENSE).

## Author

Created by [Romaric DOVI](https://github.com/romaricvivien65) — built with Claude Code.
