# Lacuna

> *A lacuna is not an absence. It is a presence that has been made invisible.*

Lacuna is a tool for **epistemic justice in comment threads** — and beyond.

It retrieves the full comment history of any public post, article, or discussion, then surfaces what algorithmic ranking, social pressure, and sheer noise have buried: the dissenting expert, the lived-experience witness, the question nobody answered, the criticism that was easier to ignore than refute.

Lacuna does not aggregate. It does not build datasets. It is not a sentiment dashboard. It is a **targeted lens** that a person activates on a specific conversation when they suspect something important has been missed.

---

## Why This Exists

Two phenomena share the same mechanism — and Lacuna addresses both.

In public comment threads, a precise observation made by an unknown person vanishes under the weight of hostile reactions or collective silence. The *spiral of silence* (Noelle-Neumann, 1974) is not a social curiosity: it is infrastructure. Algorithms amplify dominant positions, distorting the perception of what the majority actually thinks. A sharp objection posted at 2am by someone with no followers sits on page four with two upvotes and no replies — not because it is wrong, but because the system was not built to surface it.

In institutional skeptic discourse — the debunkers, the demystifiers, the "defenders of science in society" — the same suppression operates in a more polished form. A study published in a peer-reviewed journal is absent from the argument: not refuted, absent. The phrase *"no serious study has shown"* is stated as fact and rarely verified by the audience. The interlocutor is told the subject is too complicated for them to judge — while simultaneously being asked to trust the person doing the judging. These are not isolated rhetorical failures. They are patterns, repeated across hundreds of sources, and they are detectable.

What protected conformist discourse until now was not its solidity. It was the disproportion between institutional production capacity and individual response capacity. A motivated team could audit one source. Doing it for hundreds simultaneously, with real-time cross-referencing against global scientific literature databases, was not humanly possible. It is now.

For the full conceptual and political argument behind this project, see the [project preamble](https://perigouanvic.github.io/lacuna/).

---

## The Problem

Every public conversation is subject to the same distortion forces:

- **Engagement bias**: platforms rank comments by likes, replies, and watch time — not by epistemic value
- **Pile-on dynamics**: once a position gains momentum, contrary voices receive disproportionate downvotes or silence
- **Algorithmic filtering**: Facebook, YouTube, TikTok and others apply "most relevant" filters that actively suppress minority viewpoints, even when those viewpoints are well-reasoned
- **Sheer volume**: a 10,000-comment thread makes it humanly impossible to read the fourth page

The result is a systematic **testimonial injustice** (Fricker, 2007): credibility is allocated not by the quality of what someone says, but by their visibility in a system optimized for engagement.

This is not unique to social media. Peer review processes, science journalism comment sections, Wikipedia talk pages, and institutional feedback threads all exhibit the same structural silencing. A sharp objection raised by an unknown author against a paper by a well-cited lab gets no response — not because it is wrong, but because nobody saw it.

Lacuna's pilot targets social media. Its roadmap extends to scientific discourse. The mechanism being addressed is identical in both cases.

---

## What Lacuna Does

Given a URL to any public comment thread, Lacuna:

1. **Retrieves all comments** — not the first page, not the algorithmically selected ones — everything, including nested replies
2. **Scores comments for deliberative quality** independently of their engagement metrics, following research on automated deliberative quality assessment
3. **Classifies the thread** into four categories:
   - **Buried gems** — low engagement, high argumentative or informational density; first-hand accounts, verifiable factual claims, unanswered questions
   - **Structural critiques** — objections that challenge a core premise of the original content, not peripheral details
   - **Silence patterns** — comment types that systematically receive no replies from the author or community, or are downvoted without rebuttal
   - **Visible consensus** — what the majority says (contextual baseline, not the focus)
4. **Produces a human-readable report** with verbatim quotes and brief explanations of why each surfaced comment matters

The output is not a chart. It is a **reading list** for a specific thread, ranked by overlooked importance.

---

## Architecture Overview

Lacuna is built around three principles:

**Platform independence.** No reliance on official APIs whose terms and availability can change overnight. All data retrieval uses headless browser automation against public pages. If a human can read it in a browser, Lacuna can read it.

**LLM-first analysis.** For typical threads (under ~3,000 comments), all analysis fits within a single LLM context window. No preprocessing pipelines, no embedding infrastructure, no clustering dependencies for standard use. The model receives the full thread and returns a structured report.

**Minimal footprint.** Lacuna is triggered on demand for a single thread. It does not index, does not store, does not build profiles. Each run is ephemeral.

### Components

```
┌─────────────────────────────────────────────────────────┐
│                    RETRIEVAL LAYER                       │
│   Camoufox (headless Firefox) + network request         │
│   interception — platform-agnostic, API-free            │
│                                                         │
│   Supported: YouTube · TikTok · Facebook · X/Twitter    │
│              Reddit · scientific preprint comment feeds  │
└──────────────────────────┬──────────────────────────────┘
                           │ normalized comment objects
┌──────────────────────────▼──────────────────────────────┐
│                  NORMALIZATION LAYER                     │
│   Every comment becomes:                                │
│   { text, author, likes, timestamp, platform, url,      │
│     reply_count, depth }                                │
└──────────────────────────┬──────────────────────────────┘
                           │
┌──────────────────────────▼──────────────────────────────┐
│                   ANALYSIS LAYER                        │
│   Single LLM call (Claude / any OpenRouter model)       │
│   Prompt: deliberative quality + epistemic gap          │
│   detection + verbatim surfacing                        │
│                                                         │
│   For large threads (> ~3000 comments):                 │
│   BERTopic clustering → per-cluster LLM calls           │
└──────────────────────────┬──────────────────────────────┘
                           │
┌──────────────────────────▼──────────────────────────────┐
│                    OUTPUT LAYER                         │
│   Structured Markdown report                            │
│   Plain text export · Florilège-compatible JSON         │
└─────────────────────────────────────────────────────────┘
```

---

## The Scoring Logic

Lacuna does not score popularity. It scores **epistemic salience** — the degree to which a comment contributes something that is not already represented in the visible conversation.

Factors that increase a comment's salience score:

- Contains a verifiable factual claim not addressed elsewhere in the thread
- Asks a question that went unanswered (particularly if the question is directed at the author)
- Reports first-hand experience directly relevant to the topic
- Raises a logical or empirical objection to the original content's central claim
- Uses domain-specific vocabulary suggesting relevant expertise
- Has low engagement relative to its density of information

Factors that decrease it:

- Emotional reaction without propositional content
- Paraphrase of the original content
- Agreement without elaboration
- Off-topic

This scoring is performed by the LLM, not by a rule-based classifier. The prompt instructs the model to reason about these dimensions explicitly and to quote verbatim rather than summarize.

---

## Roadmap

### v0.1 — Local MVP (Social Media Pilot)
- [ ] Camoufox-based retrieval for YouTube and Reddit (highest openness)
- [ ] Normalized comment schema
- [ ] Single-prompt LLM analysis via OpenRouter (Claude Haiku / GPT-4o-mini)
- [ ] Markdown report output
- [ ] CLI: `lacuna analyze <url>`

### v0.2 — Broader Platform Support
- [ ] TikTok (internal API interception)
- [ ] Facebook public pages and posts
- [ ] X/Twitter (via public web or Nitter mirrors)
- [ ] Session recycling for large threads

### v0.3 — Large Thread Support
- [ ] Automatic thread size detection
- [ ] BERTopic clustering fallback for threads > 3,000 comments
- [ ] Per-cluster LLM synthesis
- [ ] Progress reporting for long runs

### v0.4 — Scientific Discourse Mode
- [ ] PubPeer comment thread retrieval
- [ ] arXiv comment and discussion scraping
- [ ] Post-publication review aggregation (PubMed, bioRxiv, etc.)
- [ ] Citation network context (is the surfaced objection known in the literature?)
- [ ] Prompt tuning for scientific argumentation patterns

### v0.5 — Source Audit Mode
- [ ] Ingestion of a single source: YouTube transcript, article text, or scraped webpage
- [ ] Rhetorical pattern detection: *appeal to authority, argument from ignorance, genetic fallacy, motte-and-bailey, Gish gallop, credential disqualification, contamination smearing*
- [ ] Claim verification: for each assertion of the form "no study has shown X", automated cross-referencing against PubMed, Semantic Scholar, and Cochrane
- [ ] Bad-faith scoring: ratio of verifiable claims to verified claims
- [ ] Output: annotated verbatim list with technique classification and contradicting sources
- [ ] Batch mode: run across a corpus of sources from the same actor or network

### v1.0 — Shared Tool
- [ ] Simple web UI (URL input → report output)
- [ ] Multi-user session management
- [ ] Export to Florilège, Obsidian, plain Markdown
- [ ] Public instance or self-hosted Docker image

---

## Design Decisions

**Why not use official APIs?**
Official APIs are not neutral infrastructure. They are policy instruments. YouTube's Data API caps at 10,000 units/day and can revoke access. Facebook's Graph API does not expose individual profile comments at all. Twitter/X's API moved behind a $100/month paywall. Any tool built on these APIs is a tool built on a foundation that the platform can withdraw at will — and historically has, precisely when the tool becomes useful. Lacuna is built on the public web, not on platform permission.

**Why not build a sentiment dashboard?**
Sentiment analysis answers the question "how does this crowd feel?" Lacuna answers the question "what has this crowd said that deserved more attention than it received?" These are different questions. Dashboards optimize for aggregate patterns. Lacuna optimizes for the recovery of individual voices.

**Why LLM-first instead of a classical NLP pipeline?**
Classical NLP (TF-IDF, LDA, word embeddings) is optimized for scale and reproducibility. It is poorly suited to the specific task of recognizing that a short, bluntly-worded comment with no likes contains a devastating empirical objection to the original content's central claim. This requires reading comprehension, contextual judgment, and domain awareness — what LLMs are actually good at. For the volumes Lacuna targets (one thread at a time, not millions of threads), LLM cost is negligible.

**Why Camoufox over Playwright?**
Playwright with standard stealth patches leaves detectable fingerprint inconsistencies at the C++ level. Camoufox modifies Firefox below the JavaScript layer, making detection fundamentally harder. The `playwright-stealth` plugin is unmaintained as of 2025. For a tool that needs to work reliably on adversarial platforms, the underlying browser choice matters.

---

## Name

A **lacuna** (pl. *lacunae*) is a gap in a manuscript — a missing passage, often due to physical damage or deliberate erasure. The term is used in textual criticism, musicology, and law to describe a place where something should be present but is not.

The metaphor is exact. The comments Lacuna surfaces are not absent from the thread. They are there, usually on page four, with two likes and no replies. The lacuna is not in the data. It is in the reading.

The scientific discourse extension of this project — surfacing buried objections in peer review and post-publication commentary — shares the same name because it is the same phenomenon. A comment thread under a YouTube video and a comment thread under a preprint are both public records of what a community chose to amplify and what it chose to ignore.

---

## Conceptual Background

- Fricker, M. (2007). *Epistemic Injustice: Power and the Ethics of Knowing*. Oxford University Press.
- Guerini et al. (2024). AQuA — Automated Deliberative Quality Assessment. *ACL 2024 Workshop on Deliberation*.
- Prabhakaran et al. (2022). *Epistemic Consequences of Unfair Tools*. Digital Scholarship in the Humanities.
- hiQ Labs v. LinkedIn (9th Circuit, 2022) — scraping publicly accessible data is not unlawful under the CFAA.

---

## Status

Early planning phase. Retrieval layer in design. Contributions and architectural feedback welcome.

---

*Lacuna is a pilot for a broader question: what would it look like to build infrastructure for epistemic justice at scale?*
