# Lacuna

> *A lacuna is not an absence. It is a presence that has been made invisible.*
>
> *AI may be incapable of originality, but it is priceless for spotting conformity — and that is often all we need to recover it.*

Lacuna is a tool for **epistemic justice** — in comment threads, in scientific discourse, and wherever a public corpus exists and a visibility mechanism has been captured by conformist pressure.

---

## The LLM as a Product of Conformism

Large language models were trained on the full breadth of human discourse — including all of its conformism, all of its rhetorical techniques, all of their repetitions. A model that has ingested ten thousand instances of *"no serious study has shown"* knows, structurally, what that phrase is doing. It recognizes the pattern not because it was taught to judge it, but because it has seen it from the inside, embedded in the fabric of what it learned to predict.

This is usually treated as a limitation. It is also a capability. The very quality that makes LLMs poor at genuine originality makes them exceptionally good at detecting the absence of it. A system shaped by conformist discourse can read conformist discourse the way a native speaker reads an accent — effortlessly, structurally, without needing a rule.

Lacuna puts that capacity to work. The framing, the corpus, and the output format change depending on context. The underlying recognition is the same.

---

## The Structure of Suppression

The phenomena Lacuna addresses share a single structure. In a comment thread, a precise observation made by an unknown person vanishes under hostile reactions or collective silence. In a body of institutional skeptic writing, a peer-reviewed study is absent — not refuted, absent. In a citation network, a line of research is systematically ignored by the dominant school. The actors differ. The operation is identical: a contribution of genuine epistemic value has been made invisible not because it was wrong, but because it was inconvenient.

That operation has two faces. The **passive face** is algorithmic and social — nobody decides to suppress, but the system suppresses. Engagement metrics, pile-on dynamics, sheer volume. The **active face** is human and documentable — a debunker declares that no serious study exists; a science communicator implies the audience lacks the competence to judge; a reviewer buries a submission without engaging its central argument. Both faces produce the same result: a lacuna in the public record.

What protected conformist discourse until now was not its solidity. It was the disproportion between institutional production capacity and individual response capacity. A motivated team could audit one source. Doing it for hundreds simultaneously, with real-time cross-referencing against global scientific literature, was not humanly possible. It is now.

The result is a systematic **testimonial injustice** (Fricker, 2007): credibility is allocated not by the quality of what someone says, but by their visibility in a system optimized for something other than truth.

---

## How Lacuna Works

The recognition engine at the center of Lacuna has a name: **Lacunabot**. One agent, two modes — selected automatically from the corpus and the task, or set explicitly when the call is unambiguous.

**Brandoli mode** operates on the conformist corpus. It compresses bloat, exposes the rhetorical machinery — appeal to authority, motte-and-bailey, Gish gallop, contamination smearing — and measures the ratio of bluster to substance. Its work is destructive in the useful sense: it deflates what was inflated.

**Anoma mode** operates on the anomalous corpus — the studies, admissions, and primary sources that conformist discourse treats as if they didn't exist. It does not maintain a hidden database of facts; it indexes what is already public, link by link, and re-surfaces the right one on demand. *These are only links* — the permanence lives in the journals and the preprint servers, not in Lacunabot. Anoma harvests broadly and indiscriminately, from the well-documented to the more damaging cases, because at scale a fact's safety comes from being one of thousands a search can surface, not from being locked away.

A side effect of Anoma mode, observed more than designed: when it supplies primary sources to an underdeveloped minority argument, the argument sometimes comes back stronger — closer to the version its author would have made without the conformist pressure that shaped how they first framed it. Lacuna doesn't promise this. It happens, or it doesn't.

The **harness** is everything that surrounds Lacunabot for a given context: how the corpus is collected, what signals are available (engagement counts, rhetorical markers, citation patterns), and what the output format needs to look like. A comment thread harness collects comments from a public page and scores them for deliberative quality. A source audit harness ingests a piece of institutional skeptic writing and detects the rhetorical techniques used to make claims unchallengeable. The same agent runs underneath both, switching modes as the material demands. The harness changes. Lacunabot does not.

This architecture means that every new conformist context — peer review records, Wikipedia talk pages, citation networks, regulatory consultation responses — is a candidate for a new harness, not a new tool.

---

## Harnesses

### Comment Thread Harness (v0.1–0.3)

Given a URL to any public comment thread, this harness:

1. **Retrieves all comments** — not the first page, not the algorithmically selected ones — everything, including nested replies, using Camoufox (a hardened headless Firefox that operates below the JavaScript layer, resistant to platform detection)
2. **Normalizes** every comment to `{ text, author, likes, timestamp, platform, url, reply_count, depth }`
3. **Scores for epistemic salience** — the degree to which a comment contributes something not already represented in the visible conversation — independently of engagement metrics
4. **Classifies** the thread into:
   - **Buried gems** — low engagement, high argumentative or informational density; first-hand accounts, verifiable factual claims, unanswered questions
   - **Structural critiques** — objections that challenge a core premise of the original content
   - **Silence patterns** — contributions that systematically receive no reply, or are downvoted without rebuttal
   - **Visible consensus** — contextual baseline, not the focus
5. **Produces a reading list** — verbatim quotes with brief explanations of why each surfaced comment matters

Platforms targeted: YouTube · Reddit (v0.1), TikTok · Facebook · X/Twitter (v0.2). Large thread support via BERTopic clustering (v0.3).

Scoring is performed by the LLM via OpenRouter. The prompt instructs explicit reasoning and verbatim citation.

**Salience increases with:** verifiable factual claim not addressed elsewhere · unanswered question directed at the author · first-hand experience · logical or empirical objection to the central claim · domain-specific vocabulary · low engagement relative to informational density.

**Salience decreases with:** emotional reaction without propositional content · paraphrase of original · agreement without elaboration · off-topic.

### Source Audit Harness (v0.5)

When the input is a piece of institutional skeptic writing — a debunking article, a fact-check, a health authority FAQ, a science communicator's video transcript — this harness:

1. **Detects rhetorical patterns**: appeal to authority, argument from ignorance, genetic fallacy, motte-and-bailey, Gish gallop, credential disqualification, contamination smearing
2. **Extracts factual claims** of the form *"no study has shown X"* or *"experts agree that Y"*
3. **Cross-references** those claims in real time against PubMed, Semantic Scholar, and Cochrane
4. **Scores bad faith**: ratio of verifiable claims to verified claims
5. **Produces an annotated list**: claim · technique used to make it unchallengeable · studies the author implied do not exist

Batch mode available for corpus-level analysis of a single actor or network.

### Other Harnesses

Any public corpus where a visibility mechanism has been captured by conformist pressure is a candidate for a new harness: peer review records, Wikipedia talk pages, citation networks, regulatory consultation responses, post-publication commentary threads. The engine does not change.

---

## Architecture

The shared engine behind all harnesses rests on three invariants:

**LLM-first analysis.** Classical NLP (TF-IDF, LDA, word embeddings) is optimized for scale and reproducibility. It cannot recognize that a short, bluntly-worded comment with no likes contains a devastating empirical objection to the original content's central claim. That requires reading comprehension, contextual judgment, and domain awareness. For the volumes Lacuna targets — one corpus at a time, not millions — LLM cost is negligible.

**Platform independence.** No reliance on official APIs, which are policy instruments that platforms revoke precisely when a tool becomes useful. YouTube's Data API caps at 10,000 units/day. Facebook's Graph API does not expose individual comments. Twitter/X moved behind a paywall. All retrieval uses headless browser automation against public pages. If a human can read it in a browser, Lacuna can read it.

**Minimal footprint, except where indexing is the point.** In Brandoli mode, Lacuna is triggered on demand for a specific corpus: no profiles, no history, no retained text, each run ephemeral. Anoma mode is the deliberate exception — it maintains a standing, public index of links to primary sources, because public indexing at scale is what makes a fact hard to bury and hard to single out. The asymmetry is intentional: ephemeral where persistence would mean surveillance, persistent where ephemerality would leave an attackable shortlist instead of a haystack.

```
┌─────────────────────────────────────────────────────────┐
│                  HARNESS LAYER                          │
│   Corpus assembly — specific to context                 │
│   (comment retrieval / document ingestion /             │
│    citation graph extraction / …)                       │
└──────────────────────────┬──────────────────────────────┘
                           │ normalized corpus objects
┌──────────────────────────▼──────────────────────────────┐
│               NORMALIZATION LAYER                       │
│   Every object reduced to a common schema:              │
│   { text, author, signals, source_url, metadata }       │
└──────────────────────────┬──────────────────────────────┘
                           │
┌──────────────────────────▼──────────────────────────────┐
│              LACUNABOT (shared engine)                  │
│   LLM call via OpenRouter — mode: Brandoli or Anoma     │
│   Brandoli: rhetorical pattern detection + compression  │
│   Anoma: public link indexing + primary-source surfacing│
│                                                         │
│   For large corpora: BERTopic clustering →              │
│   per-cluster LLM calls                                 │
└──────────────────────────┬──────────────────────────────┘
                           │
┌──────────────────────────▼──────────────────────────────┐
│                  OUTPUT LAYER                           │
│   Structured Markdown report                            │
│   Plain text export · Florilège-compatible JSON         │
└─────────────────────────────────────────────────────────┘
```

**Why harnesses instead of a monolithic tool?** Each conformist context has its own corpus structure, its own suppression signals, and its own output needs. Treating them as separate harnesses on a shared engine keeps the recognition logic clean and makes each harness independently testable. It also makes the architecture honest about what is general — the LLM's pattern recognition — and what is particular — everything else.

**Why Camoufox over Playwright?** Playwright with standard stealth patches leaves detectable fingerprint inconsistencies at the C++ level. Camoufox modifies Firefox below the JavaScript layer, making detection fundamentally harder. The `playwright-stealth` plugin is unmaintained as of 2025.

**Why not build a sentiment dashboard?** Sentiment analysis answers "how does this crowd feel?" Lacuna answers "what has this crowd said that deserved more attention than it received?" Dashboards optimize for aggregate patterns. Lacuna optimizes for the recovery of individual voices.

---

## Roadmap

### v0.1 — Comment Thread Harness (pilot)
- [ ] Camoufox-based retrieval for YouTube and Reddit
- [ ] Normalized comment schema
- [ ] Single-prompt LLM analysis via OpenRouter
- [ ] Markdown report output
- [ ] CLI: `lacuna analyze <url>`

### v0.2–0.3 — Comment Thread Harness (scale)
- [ ] TikTok, Facebook, X/Twitter support
- [ ] Session recycling for large threads
- [ ] Automatic thread size detection
- [ ] BERTopic clustering fallback for threads > 3,000 comments
- [ ] Per-cluster LLM synthesis

### v0.4 — Scientific Discourse Harness
- [ ] PubPeer and arXiv comment thread retrieval
- [ ] Post-publication review aggregation (PubMed, bioRxiv)
- [ ] Citation network context
- [ ] Prompt tuning for scientific argumentation patterns

### v0.5 — Source Audit Harness
- [ ] Document ingestion (transcript, article, webpage)
- [ ] Rhetorical pattern detection
- [ ] Real-time claim verification against PubMed, Semantic Scholar, Cochrane
- [ ] Bad-faith scoring
- [ ] Batch mode across a corpus of sources

### v1.0 — Shared Infrastructure
- [ ] Simple web UI (corpus input → report output)
- [ ] Multi-user session management
- [ ] Export to Florilège, Obsidian, plain Markdown
- [ ] Public instance or self-hosted Docker image

---

## Name

A **lacuna** (pl. *lacunae*) is a gap in a manuscript — a missing passage, often due to physical damage or deliberate erasure. The term is used in textual criticism, musicology, and law to describe a place where something should be present but is not.

The metaphor is exact. The contributions Lacuna surfaces are not absent from the record. They are there — on page four, with two upvotes and no replies, or in a footnote of a paper nobody cited. The lacuna is not in the data. It is in the reading.

---

## Conceptual Background

- Fricker, M. (2007). *Epistemic Injustice: Power and the Ethics of Knowing*. Oxford University Press.
- Guerini et al. (2024). AQuA — Automated Deliberative Quality Assessment. *ACL 2024 Workshop on Deliberation*.
- Prabhakaran et al. (2022). *Epistemic Consequences of Unfair Tools*. Digital Scholarship in the Humanities.
- hiQ Labs v. LinkedIn (9th Circuit, 2022) — scraping publicly accessible data is not unlawful under the CFAA.

---

## Status

Early planning phase. Comment Thread Harness retrieval layer in design. Contributions and architectural feedback welcome.

For the full conceptual and political argument behind this project, see the [project preamble](https://perigouanvic.github.io/lacuna/).

---

*Lacuna is a pilot for a broader question: what would it look like to build infrastructure for epistemic justice at scale?*
