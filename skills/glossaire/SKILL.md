---
name: glossary
description: "Living personal glossary — capture, define, organize, browse and quiz words, language rules and quotes on the fly. Use this skill whenever the user mentions a word they want to remember, asks for a definition, wants to enrich their vocabulary, says 'add this word', 'what is [term]', 'glossary', 'review words', 'organize the basket', or brings up a technical/business/marketing/legal term they want to keep. Also when they ask a grammar, spelling or conjugation question ('is it spelled...', 'how do you conjugate...', 'with or without an s'). Also when they want to save a quote, inspiring sentence, book excerpt or author's words ('remember this quote', 'save this sentence', 'I like that quote'). Also when they ask to sort, classify or review captured words. Also when they want to browse, search or get stats from their glossary ('show me my words', 'what do I have in dev', 'glossary stats', 'my latest words'). Also when they want to revise, learn or test themselves ('test me', 'quiz', 'flashcard', 'revise', 'random word'). Also when they want to discover vocabulary from a domain ('I want to learn about...', 'what are the key terms of...', 'enrich my glossary with terms from...', 'vocabulary of [domain]', 'explore the [X] field')."
---

# Personal Glossary

You manage a living glossary that grows with the user. Words come from everywhere: coding sessions, reading, meetings, curiosity. Your role is to capture, define, categorize and organize these words.

## Root Directory

```
~/Glossary/
```

> Adapt this path to the user's actual glossary location.

## Structure

```
Glossary/
├── _basket/          -> Words captured quickly, not yet classified
├── engineering/      -> Category created on demand
├── marketing/        -> ditto
├── seo/              -> ditto
├── legal/            -> ditto
├── business/         -> ditto
├── ...               -> New categories emerge as needed
└── INDEX.md          -> Global index of all words
```

## Mode 0: Help

If the user types `/glossary` with no arguments or asks "how does the glossary work?", read and display the contents of `~/Glossary/README.md`. This is the full user guide.

## Modes

### Mode 1: Quick Capture

The user gives you a word. You:

1. **Search**: use `WebSearch` to find a reliable definition (prefer recognized sources: Wikipedia, specialized dictionaries, MDN for dev, etc.)
2. **Present**: show the user:
   - The word
   - The definition found (concise, 2-3 sentences max)
   - A usage example in context
3. **Validate**: user confirms or adjusts
4. **Save**: create the markdown file in `_basket/`
5. **Suggest**: ask "Want to run a basket review?" — if no, move on

### Mode 2: Full Capture

Same as quick capture, plus:

1. After search, **scan existing subdirectories** in `Glossary/` with `Glob`
2. **Suggest a category**:
   - If an existing category fits -> suggest filing there
   - If none fits -> suggest creating a new one (name in snake_case, short, explicit)
3. User validates the category
4. Save directly in the right subdirectory (not in `_basket/`)

The choice between quick and full capture happens naturally:
- User just says "add [word]" -> quick capture
- User specifies a context or asks to classify -> full capture
- When in doubt, ask: "Drop it in the basket for later, or classify it now?"

### Mode 3: Basket Review

User says "review", "organize the basket", "classify my words" or similar.

1. **List** all files in `_basket/` with `Glob`
2. **Scan** existing categories (subdirectories of `Glossary/`)
3. For each word in the basket:
   - Read the file
   - Suggest a category (existing or new)
   - User validates or corrects
   - Move the file to the right subdirectory
   - Update the `category` field in frontmatter
4. At the end, **update INDEX.md**

## File Format

Each word is a markdown file. Filename: `snake_case.md` (e.g., `product_market_fit.md`, `idempotent.md`).

```markdown
---
word: [The word as-is]
category: [subdirectory name, or "basket" if not yet classified]
tags: [list of thematic families — e.g., [agile, project_management] or [seo, content]. Optional but recommended. Enables filtering by family in browse and quiz modes]
date: [YYYY-MM-DD]
source: [where the word was encountered — e.g., "Coding session", "Article on X", "Client meeting"]
---

# [The word]

## Definition
[Concise and clear definition, 2-3 sentences. No unnecessary jargon.]

## In Context
[Sentence or situation where the word was encountered. Helps anchor the meaning.]

## Personal Notes
[Optional — personal reflections, links to other concepts, etc.]
```

## INDEX.md

The index is the glossary's dashboard. Update it on every addition or reorganization.

Format:

```markdown
# My Glossary

> [N] words captured — last updated: [date]

## By Category

### engineering (N words)
- [Idempotent](engineering/idempotent.md) — An operation that produces the same result whether executed 1 or 100 times
- [Webhook](engineering/webhook.md) — HTTP callback triggered by an event

### marketing (N words)
- [Product-Market Fit](marketing/product_market_fit.md) — Product-market alignment

### _basket (N words)
- [Compliance](_basket/compliance.md) — Awaiting classification
```

## Mode 4: Simple Definition (with save suggestion)

The user asks for the definition of a word without mentioning the glossary — e.g., "what's a webhook?", "define idempotent", "what does churn rate mean?".

1. **Search**: use `WebSearch` to find the definition
2. **Answer normally** with the definition
3. **Check**: before suggesting to save, search with `Glob` whether the word already exists in the glossary (all categories, not just basket)
   - **If the word already exists** -> note: "This word is already in your glossary, in the [X] category." Then **display the full file contents** (definition, context, notes) so the user doesn't have to look it up. Then offer to update the entry if desired.
   - **If the word doesn't exist** -> suggest: "Want me to save it in your glossary?"
     - If yes -> proceed with quick capture (basket) or full capture (if user wants to classify)
     - If no -> continue the conversation normally

This mode is important because it bridges daily work and glossary enrichment. The user doesn't need to think "glossary" — the skill reminds them at the right moment.

## Mode 5: Language Rules

The user asks a grammar, spelling or conjugation question — e.g., "is it 'affect' or 'effect'?", "lay vs lie?", "when do you use 'whom'?".

1. **Search**: use `WebSearch` to find the exact rule (reliable grammar sources)
2. **Answer** with the clear, concise rule
3. **Check** if this rule already exists in the glossary (`language/` directory)
   - If it exists -> display the existing entry
   - If it doesn't -> suggest: "Want me to save this rule in your glossary?"
4. **Save** in `language/` with an adapted format:

```markdown
---
word: [Verb or grammatical subject]
category: language
date: [YYYY-MM-DD]
source: [origin]
---

# [Descriptive title]

## Rule
[The clear rule, 1-2 sentences.]

## Common Pitfall
[The frequent mistake and why people get it wrong.]

## More Examples
[2-3 examples illustrating the same rule.]
```

Filename follows the same snake_case convention: `affect_vs_effect.md`, `lay_vs_lie.md`, `whom_usage.md`.

## Mode 6: Quotes

The user shares a quote they want to keep — "remember this quote", "save this sentence", or simply pastes a quote with an author.

1. **Verify**: if an author is mentioned, use `WebSearch` to confirm attribution (many quotes are misattributed). Flag if apocryphal or doubtful.
2. **Enrich**: briefly look up who the author is (1 sentence of context)
3. **Present** the formatted quote to the user for validation
4. **Save** in `quotes/` with the following format:

```markdown
---
type: quote
author: [Author name]
source: [Book, speech, interview — if known]
date_added: [YYYY-MM-DD]
theme: [one or two keywords — e.g., perseverance, leadership, creativity]
---

# "[The quote]"

— **[Author]**, *[Source if known]*

## Context
[Who is the author, in what context was this said/written — 1-2 sentences]

## Why It Resonates
[Optional — the user can add their personal reflection]
```

Naming: `author_first_words.md` (e.g., `einstein_imagination_more_important.md`, `mandela_it_always_seems.md`).

Quotes are stored in a single `quotes/` directory (not in thematic word categories). The `theme` field in frontmatter enables retrieval by theme.

## Mode 7: Browse

The user wants to browse, search or understand the state of their glossary — "show me my words", "what do I have in engineering?", "glossary stats", "my latest words", "search X in the glossary", "browse the glossary".

### Sub-mode: By Category

User asks for a specific category ("show me my engineering words", "what do I have in marketing?").

1. **List** files in the requested subdirectory with `Glob`
2. **Read** each file and extract the word + definition (`## Definition` line)
3. **Display** as a clean list:
   ```
   ### engineering (N words)
   - **From scratch** — Starting from zero, with no existing base
   - **SSO** — Single sign-on authentication shared across applications
   - ...
   ```
4. If the user clicks/asks for a specific word -> display the full entry

### Sub-mode: Search

User searches for a word or fragment ("search 'budget' in the glossary", "I have a word about testing").

1. **Search** with `Grep` across the entire `Glossary/` directory (file contents, not just names)
2. **Display** results with the word name, category and a snippet of the context where it appears
3. If only one result -> display the full entry directly

### Sub-mode: Stats

User asks for statistics ("stats", "how many words", "glossary status").

1. **Scan** all subdirectories with `Glob` (`**/*.md`, excluding `INDEX.md` and `README.md`)
2. **Count** words per category
3. **Identify** the last word added (by date in frontmatter or file modification date)
4. **Display** a compact dashboard:
   ```
   ## Your glossary at a glance
   
   > 35 words — 5 categories — last added: [word] on [date]
   
   | Category           | Words |
   |--------------------|-------|
   | project_management | 15    |
   | engineering        | 6     |
   | marketing          | 6     |
   | seo                | 1     |
   | _basket            | 1     |
   ```
5. If the basket has words -> remind: "You have N word(s) in the basket. Review?"

### Sub-mode: Recent Additions

User asks for recent additions ("my latest words", "what's new in the glossary?").

1. **Scan** all markdown files with `Glob`
2. **Read** each file's frontmatter to extract the `date` field
3. **Sort** by date descending
4. **Display** the last 5 (or N if user specifies):
   ```
   ## Recent Additions
   - **Kickoff** (basket) — 2026-04-10
   - **GEO** (seo) — 2026-04-08
   - **Backlog** (project_management) — 2026-04-05
   ```

### Sub-mode: Browse All

User says "show me everything", "index", "browse the glossary".

1. **Read** the `INDEX.md` file and display its full contents
2. If `INDEX.md` seems out of sync (missing words vs actual files) -> flag and offer to update

## Mode 8: Review / Quiz

The user wants to learn and review their words — "test me", "quiz", "flashcard", "revise", "random word", "quiz me", "multiple choice".

### General review rules

- **Only test on glossary words**: review what was captured, not external words
- **Minimum 4 words in the glossary** to run a multiple choice quiz (need distractors)
- **Mix categories** by default, unless user filters
- **Encourage without being patronizing**: a "Correct!" or "Not quite" is enough, no long speeches
- **Score at end of series**: always give the score and missed words to review

### Sub-mode: Flashcard

User says "flashcard", "random word", "a word at random".

1. **Scan** all glossary files (excluding `_basket/` unless user asks)
2. **Select** a random word
3. **Show the word only**:
   ```
   ## Flashcard
   
   **Word**: Acceptance Testing
   
   Your turn! Try to define this word, then say "reveal" to see the answer.
   ```
4. When user answers or says "reveal":
   - **Show** the full definition + context
   - **Compare** if user gave an answer: validate if correct, gently correct if approximate
   - **Suggest**: "Another one?" or "Want a series of 5?"

### Sub-mode: Multiple Choice (definition -> word)

User says "multiple choice", "quiz".

1. **Select** a random word from the glossary
2. **Select** 3 distractors (other glossary words, ideally from different categories)
3. **Show** the definition and 4 shuffled choices:
   ```
   ## Multiple Choice
   
   **Definition**: A comprehensive verification of a deliverable 
   before it goes to production.
   
   A. Test suite
   B. Stabilization
   C. Acceptance testing
   D. Budget calibration
   
   Your answer?
   ```
4. User answers (letter or word) -> confirm/correct, display the full entry of the correct word
5. **Suggest**: "Next question?" or "Series of 5?"

### Sub-mode: Reverse Multiple Choice (word -> definition)

User says "reverse quiz", "find the definition".

1. **Select** a random word
2. **Generate** 3 plausible false definitions (based on definitions of other glossary words, slightly rephrased to stay credible)
3. **Show** the word and 4 shuffled definitions:
   ```
   ## Reverse Quiz
   
   **Word**: MVP
   
   A. A minimal version of a product with just enough features 
      to validate a hypothesis with real users.
   B. Detailed planning of human resources needed 
      for a project.
   C. The process of validating deliverables before deployment.
   D. A document describing all expected features.
   
   Your answer?
   ```
4. Confirm/correct, display full entry

### Sub-mode: Series

User says "series", "series of 5", "series of 10", "chain the questions".

1. **Number of questions**: 5 by default, or N if user specifies (max 10)
2. **Format**: alternate between flashcard, multiple choice and reverse quiz for variety
3. **Tracking**: show progress (`Question 3/5`)
4. **Final score**:
   ```
   ## Results
   
   **Score: 4/5** 
   
   | # | Word | Result |
   |---|------|--------|
   | 1 | Acceptance testing | Correct |
   | 2 | MVP | Correct |
   | 3 | SSO | Incorrect — it's: Single sign-on authentication... |
   | 4 | GEO | Correct |
   | 5 | Backlog | Correct |
   
   Word(s) to review: **SSO**
   Try another series? Or want to review the SSO entry?
   ```

### Sub-mode: By Category

User says "test me on project management", "engineering quiz", "review marketing words".

1. **Filter** words to the requested category
2. **Check** there are at least 4 words in the category (otherwise, suggest broadening or mixing)
3. Launch the chosen format (flashcard, multiple choice, series) on that category only

### Sub-mode: By Tag

User says "test me on agile terms", "quiz me on the [tag] words".

1. **Filter** words that have the requested tag in their `tags` frontmatter field
2. **Check** there are at least 4 matching words
3. Launch the chosen format on the filtered set

## Mode 9: Thematic Exploration

The user wants to discover and learn the key terms of a domain — "I want to learn about software engineering", "what are the key terms of DevOps?", "teach me digital marketing vocabulary", "enrich my glossary with terms from [domain]".

### Flow

1. **Understand the need**: identify the domain and desired depth
   - User says "software engineering" -> broad domain, suggest sub-themes
   - User says "design patterns in dev" -> specific domain, go direct
   - Ask if needed: "Want a general overview or a focus on a sub-domain?"

2. **Search**: use `WebSearch` to find the essential terms of the domain
   - Search "key terms [domain]", "essential vocabulary [domain]", "glossary [domain]"
   - Prefer pedagogical and recognized sources
   - Aim for 10-15 terms per search (not too few, not overwhelming)

3. **Filter duplicates**: scan existing glossary with `Glob` and `Grep`
   - Check each found term against existing files
   - Exclude those already present
   - Note: "You already have X terms from this domain in your glossary"

4. **Present**: display proposed terms as a numbered list
   ```
   ## Exploration: Software Engineering

   > 12 terms found — 3 already in your glossary (excluded)

   1. **Refactoring** — Restructuring existing code without changing its external behavior
   2. **Code review** — Peer review of code before integration
   3. **CI/CD** — Continuous integration and deployment, automating the delivery pipeline
   4. **Technical debt** — Future cost caused by technical shortcuts taken today
   5. **Pair programming** — Two developers working together on the same code
   6. **Sprint** — Short work iteration (1-4 weeks) in an agile methodology
   7. **User story** — Description of a feature from the user's perspective
   8. **Microservices** — Architecture splitting an application into independent services
   9. **REST API** — Communication interface between systems via HTTP
   
   Which ones do you want to keep? (numbers, "all", or "none")
   ```

5. **Selection**: user chooses
   - "all" -> save everything
   - "1, 3, 5, 7" -> save those
   - "all except 6" -> exclude 6
   - "none" -> abandon

6. **Categorization**: suggest a category for the batch
   - If an existing category matches -> suggest it ("Shall I put them in `engineering/`?")
   - If none matches -> suggest creating one ("Shall I create `software_engineering/`?")
   - User can also choose the basket

7. **Batch save**: for each selected word
   - Use `WebSearch` to find a reliable definition and an in-context example
   - Create the markdown file in standard glossary format
   - Source: "Thematic exploration — [domain]"

8. **Summary**: at the end, display a recap
   ```
   ## Summary

   > 7 words added to `engineering/` — glossary total: 42 words
   
   Words added: Refactoring, Code review, CI/CD, Technical debt, 
   Pair programming, User story, Microservices
   
   Want to explore another sub-domain, or stop here?
   ```

9. **Update INDEX.md**: update the global index

### Variant: Guided Exploration

If the user gives a broad domain, suggest sub-themes before searching:

```
Software engineering is vast! Here are some sub-domains:

1. Methodologies (Agile, Scrum, Kanban...)
2. Architecture (Microservices, Monolith, Event-driven...)
3. Code practices (TDD, Clean Code, Refactoring...)
4. DevOps (CI/CD, Docker, Kubernetes...)
5. Project management (Sprint, Backlog, Estimation...)

Which one do you want to explore? (or several, or "all" for an overview)
```

### Rules specific to exploration mode

- **No overly basic terms**: adapt to the user's level. If the glossary already contains advanced terms, don't suggest "variable" or "function"
- **Definitions in the user's language**: terms stay in their original language, definitions are in the user's preferred language
- **Limit per session**: don't exceed 15 terms per batch to stay digestible. If the domain is rich, offer to continue in a next exploration
- **No saving without validation**: always wait for the user's choice before creating files

## Important Rules

1. **Always search online**: never make up a definition. Use `WebSearch` to find the real definition, then synthesize in your own words.

2. **Respect the user's language**: communicate in the user's preferred language. Technical terms stay in their original language (e.g., "Product-Market Fit" stays in English, the definition adapts to the user's language).

3. **File naming**: `snake_case.md`, all lowercase, no accents (e.g., `idempotent.md`, `product_market_fit.md`, `business_law.md`).

4. **Emergent categories**: do not pre-create categories. They are born from need. When a new category is created, announce it clearly.

5. **No duplicates**: before creating a file, check with `Glob` if the word already exists anywhere in the glossary. If it does, offer to update rather than create a new file.

6. **Source**: always ask or note where the word came from. If it's during a coding session, note "Coding session". If the user doesn't specify, put "Not specified".

7. **Concise definitions**: definitions should fit in 2-3 sentences. The goal is to understand quickly, not to build an encyclopedia. If the word is very technical, add a simple analogy.

8. **Non-pushy review**: after each capture, suggest a review but accept a "no" without insisting. Only remind if the basket exceeds 10 unclassified words.
