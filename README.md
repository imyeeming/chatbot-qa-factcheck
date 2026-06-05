# chatbot-qa-factcheck

A Claude skill for auditing AI chatbot responses. Instead of re-answering the question, it breaks the chatbot's output into atomic claims, classifies each one, and produces a structured verdict with source links.

## Origin

This skill was developed from hands-on experience in enterprise customer service AI QA work. Over a two-week engagement evaluating a production chatbot, I repeatedly observed failure modes that standard fact-checking missed — responses that were *mostly correct* but contained one misleading inference, one outdated claim, or one irrelevant paragraph designed to fill space when the system couldn't find the real answer.

The 9 error patterns documented in this skill (see [common-error-patterns.md](references/common-error-patterns.md)) were identified from real chatbot outputs during that work. The skill itself was written afterward to formalize those observations into a reusable auditing framework.

## What It Does

Given a chatbot response, the skill:

1. **Classifies the response type** — Is the chatbot actually answering, refusing to answer, or pivoting to unrelated content?
2. **Splits the response into atomic claims** — Mixed paragraphs get broken down so each fact and each inference is judged separately.
3. **Verifies each claim against primary sources** — With mandatory web search before any verdict. Internal model knowledge triggers search, never judgment.
4. **Assigns a verdict** — `Correct` / `Partly correct` / `Unsupported` / `Outdated` / `Incorrect`, with source links at the point of conclusion.
5. **Identifies the failure pattern** — Maps the error to one of 9 documented patterns (e.g., Fact-Inference Entanglement, Attribute Mismatch, Red Herring).

## Example

**User question:** "What material is the laptop case made of?"

**Chatbot response:** "The laptop is available in black and silver color options."

**Skill output:**

```
Response type: Direct answer

Overall verdict: Incorrect (Attribute Mismatch)

1. Claim: The laptop is available in black and silver.
   - Verdict: Correct
   - Source: [manufacturer product page]

2. Claim: This answers the question about case material.
   - Verdict: Incorrect
   - Why: The question asked about material (e.g., aluminum, polycarbonate),
     not color. The chatbot retrieved the correct entity but answered a
     different attribute. See: Attribute Mismatch pattern.

Bottom line: The stated facts are true, but the response fails because
it answers the wrong question. The case material was never mentioned.
```

See [examples/](examples/) for more worked examples.

## The 9 Error Patterns

These are the recurring failure modes observed in production chatbot QA. Full definitions, examples, root causes, and detection heuristics are in [common-error-patterns.md](references/common-error-patterns.md).

| # | Pattern | What goes wrong |
|---|---------|----------------|
| 1 | Fact-Inference Entanglement | True specs used to make an unverified conclusion sound authoritative |
| 2 | Attribute Mismatch | Right product, wrong attribute (asked material, answered color) |
| 3 | Temporal Hallucination | Outdated information presented as current |
| 4 | Ecosystem Blindness | Cross-device question answered from a single product's spec sheet |
| 5 | False Refusal | Chatbot refuses to answer despite publicly available information |
| 6 | Forced Pivot | Redirects to a different topic without answering the original question |
| 7 | Equivocation | Same term used with a different meaning than intended |
| 8 | Red Herring / ESG Shield | Irrelevant sustainability or privacy claims used to fill space |
| 9 | Over-Answering | Historical padding that misleads or contradicts the current answer |

## Key Design Decisions

**Reviewer knowledge boundary rule.** The skill explicitly prohibits using internal model knowledge to mark a claim as Incorrect or Outdated. If the chatbot mentions a newer product or version than the reviewing model's training data covers, that triggers a web search — not a verdict. This prevents false positives from stale training data.

**Step 0: Response type classification.** Before splitting claims, the skill first determines whether the chatbot is answering, refusing, or pivoting. A refusal to publicly documented information is itself a failure mode, not a safety feature.

**Source priority hierarchy.** Official docs → high-quality secondary → community evidence. Each tier is labeled. See [source-priority.md](references/source-priority.md).

## File Structure

```
chatbot-qa-factcheck/
├── README.md                          ← You are here
├── SKILL.md                           ← Main skill file (for Claude)
├── references/
│   ├── source-priority.md             ← Source ordering and verdict rules
│   └── common-error-patterns.md       ← 9 error patterns with examples
└── examples/                          ← Sample inputs and outputs (WIP)
```

## Limitations

- **No automated scoring.** This is a prompt-driven skill, not an eval pipeline with metrics. It depends on the reviewing model's judgment and search quality.
- **Not tested at scale.** The skill was developed and used in manual QA workflows (one response at a time), not batch evaluation.
- **Examples use public information.** Due to confidentiality obligations from prior work, the examples in this repo use publicly available product information rather than actual chatbot logs.
- **Single-domain origin.** The 9 error patterns were identified in consumer electronics customer service. They likely generalize to other product-support domains but have not been validated outside that context.

## Usage

### With Claude Projects or Claude Code

Place `SKILL.md` and the `references/` folder in your project. The skill triggers when you ask Claude to fact-check or QA a chatbot response.

### With other LLMs

The core workflow and error taxonomy in `SKILL.md` are written in plain English and Chinese. They can be adapted as a system prompt or evaluation rubric for any LLM that supports web search.

## License

MIT
