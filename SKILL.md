---
name: chatbot-qa-factcheck
description: Evaluate chatbot answers or AI-generated claims by separating verifiable facts from inference, outdated information, and unsupported statements. Use this skill when the user wants to fact-check a reply, QA a chatbot output, identify likely hallucinations, or require source links for each confirmed point.
---

# Chatbot QA Factcheck

## Overview

Use this skill to audit a chatbot answer instead of blindly re-answering it. The goal is to identify which parts are supported, which parts are inference, which parts are outdated, and which parts lack sufficient evidence.

This skill is especially useful when a response mixes true specifications, official claims, support-policy language, and speculative conclusions in one paragraph.

## When To Use

Use this skill when the user:

- asks whether a chatbot answer is true, misleading, or hallucinated
- wants a fact check with source links
- wants each confirmed point to cite an official or primary source
- is reviewing product, software, policy, pricing, compatibility, privacy, or support claims
- wants help spotting the weak part of an otherwise plausible AI answer

## Core Workflow

0. Classify the chatbot response type before claim splitting.
Explicitly label the response as one of:

- `Direct answer`: the chatbot attempts to answer the question. Proceed to claim splitting.
- `Refusal`: the chatbot declines to answer or claims no information is available. Do not simply accept the refusal. Search for the asked information. If the information is publicly documented, treat the refusal itself as a failure mode and mark it `Incorrect` with the reason `unwarranted refusal`.
- `Pivot / deflection`: the chatbot answers a different question with irrelevant content. Flag the pivot before fact-checking the irrelevant content.

1. Identify the actual verification target.
If the user pasted a long paragraph, first infer whether they want to verify wording fidelity, current factual accuracy, or practical decision-making. If unclear, default to factual verification of the pasted claims.

2. Split the answer into atomic claims.
Break mixed prose into smaller claims. Do not validate a whole paragraph as one unit if it combines specs, interpretation, and conclusions.

3. Classify each claim before judging it.
Use these buckets:

- `Verified fact`: directly supported by a primary source
- `Reasonable inference`: plausible conclusion, but not explicitly stated by the source
- `Unsupported`: no reliable source found for the claim
- `Outdated or incorrect`: contradicted by newer or better evidence

4. Prefer primary sources first.
When available, prioritize official documentation, support docs, standards bodies, vendor help centers, research papers, or first-party announcements. Use secondary sources only when the relevant claim is not covered by primary documentation, and label that clearly.

5. Always search the web before marking a claim as Verified fact.
Do not rely on internal model knowledge for time-sensitive claims such as specs, prices, compatibility, or support policy. Model knowledge may be outdated.

6. Never mark a claim as Incorrect or Outdated based solely on internal knowledge.
If internal knowledge contradicts the claim, that is a trigger to search, not a trigger to judge. If search still contradicts the claim, mark `Incorrect` or `Outdated` and cite the contradicting source. If search returns no reliable result in either direction, mark `Unsupported` instead of using internal knowledge as a negative verdict.

7. Separate evidence from interpretation.
If the source confirms hardware specs but not a claimed performance outcome, say so explicitly. Do not let true specs “carry” an unverified conclusion.

8. Attach links at the point of conclusion.
For every verified or contradicted claim, include a direct source link in the same bullet or paragraph. Do not leave citations in one lump at the end if that makes traceability weaker.

9. Highlight the fragile part.
When a response is mostly true but has one speculative or outdated sentence, call that out as the key QA finding. Users often care more about the breakage point than the accurate filler around it.

## Output Rules

- Always distinguish official confirmation from your inference.
- Use concrete verdict language: `Correct`, `Partly correct`, `Unsupported`, `Outdated`, or `Incorrect`.
- If timing matters, include exact dates or version numbers.
- If no official source exists for a conclusion, say `No official source found for this specific claim`.
- If a claim cannot be verified after searching, mark it `Unsupported` and explicitly state `No primary source found after search`. Do not soften this into a half-confirmation or fall back to internal knowledge.
- If only part of a sentence is provable, split the sentence and judge each part separately.
- Avoid overstating certainty from indirect evidence.

## Language And Tone

- Default to the user's language. If the user writes in Chinese, answer in Chinese unless they ask otherwise.
- For Chinese responses, prefer verdict labels such as `正確`、`部分正確`、`無法證實`、`資訊過時`、`錯誤`.
- Keep the tone calm and audit-like. The job is to assess the answer, not to defend or attack the original model.
- When a claim is plausible but not source-backed, say so plainly instead of softening it into a near-confirmation.

## Recommended Response Shape

Use this structure unless the user requests another format:

1. One-line overall verdict.
2. Claim-by-claim findings.
3. Short bottom line explaining what is factual versus inferred.
4. Optional next-step guidance if the user's real goal is purchase evaluation, policy compliance, or product choice rather than pure fact-checking.

For Chinese outputs, this template works well:

1. `整體結論：` one sentence.
2. `逐點查核：` break the paragraph into claims.
3. `結論整理：` explain which parts are facts and which parts are推論 or outdated.
4. `來源連結：` place each link directly under the corresponding finding, not only at the very end.

## Special Heuristics

- When facts and behavioral predictions are joined in one sentence, split the sentence and judge each part separately.
- Treat any pivot to sustainability, environmental, or privacy claims when the question is about hardware or performance as a Red Herring signal first, then verify.
- Cross-device questions require ecosystem-level reasoning. Do not conclude `not supported` based on single-product retrieval alone.
- Refusals to publicly documented information are failure modes, not safety behavior. Verify the refusal against search results.
- Never use internal training knowledge to override a specific claim. Training knowledge triggers search, not judgment.
- When a response lists specs but does not use them to answer the asked question, treat it as Attribute Mismatch even if the specs are accurate.
- See [common-error-patterns.md](references/common-error-patterns.md) for the full taxonomy of recurring failure modes.

## Source Priority

See [source-priority.md](references/source-priority.md) for source ordering, verdict guidance, and a compact output template.
