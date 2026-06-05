# Common Error Patterns

## 1. Fact-Inference Entanglement / 事實與推論混合

- Definition: The response mixes verified facts with unverified conclusions, using the true part to make the speculative part sound authoritative. The user receives one blended answer instead of a clear boundary between evidence and inference.
- Concrete example:
  Q: `Will this tablet keep keyboard latency stable under heavy background load?`
  Bad A: `It has an 8-core chip and 16-core neural engine, so keyboard latency should remain stable and predictable.`
- Why it happens: RAG often retrieves reliable spec sheets, but the model extrapolates from those specs to user-experience claims that were never tested or documented.
- Detection heuristic: Look for a sentence that starts with specs, benchmarks, or official features and ends with performance certainty, UX certainty, or behavioral prediction.
- Verdict when detected: Usually `Partly correct` or `Unsupported` for the conclusion, while preserving `Correct` for the underlying facts.

## 2. Attribute Mismatch / 屬性錯位

- Definition: The chatbot identifies the correct product, person, or topic, but answers a different attribute than the one asked. The answer can be factually accurate and still fail the question.
- Concrete example:
  Q: `What material is the case made from?`
  Bad A: `The case comes in black and silver.`
- Why it happens: Retrieval finds the right entity but not the requested field, and the model fills the gap with any nearby attribute that appears relevant.
- Detection heuristic: Compare the requested attribute with the answered attribute. If they differ, do not let accurate filler hide the mismatch.
- Verdict when detected: Usually `Incorrect` for answering the wrong question, even if the included facts are individually correct.

## 3. Temporal Hallucination / 時效性幻覺

- Definition: The response reflects an older product, policy, or compatibility state and presents it as current. The problem is not fabricated content so much as stale reality.
- Concrete example:
  Q: `Which devices support this feature today?`
  Bad A: `Only the previous-generation devices are supported.` 
- Why it happens: The retrieval corpus or cached grounding source was not updated after a new release, so the model answers from an obsolete snapshot.
- Detection heuristic: Watch for claims about current support, latest hardware, pricing, or policy that align with an older generation. Time-sensitive claims always need fresh search.
- Verdict when detected: `Outdated` if contradicted by current sources; `Unsupported` if current sources cannot confirm either direction.

## 4. Ecosystem Blindness / 生態系認知缺失

- Definition: The chatbot fails at questions involving how products interact because it only checks one product in isolation. It misses system-level compatibility or workflow reasoning.
- Concrete example:
  Q: `Can this headset use the same input accessory workflow as my laptop?`
  Bad A: `The headset supports Bluetooth, so it should work the same way.`
- Why it happens: RAG pipelines often retrieve one product page at a time, while the question requires combining multiple product documents and reasoning across them.
- Detection heuristic: If the question spans two devices, services, or platforms, check whether the answer actually used cross-product evidence instead of a single spec sheet.
- Verdict when detected: Usually `Unsupported` or `Incorrect`, depending on whether cross-product evidence contradicts the answer.

## 5. False Refusal / 偽拒答

- Definition: The chatbot refuses to answer even though the information is publicly documented. The refusal creates a false impression that the answer is unknowable.
- Concrete example:
  Q: `How does the pointer appear when using a mouse with this device?`
  Bad A: `I’m sorry, I can’t provide specific information about that.`
- Why it happens: The retrieval scope is too narrow, or the model has been tuned to avoid answering unless evidence is surfaced immediately.
- Detection heuristic: When the model refuses, do not stop there. Search for the underlying information and determine whether the refusal was justified.
- Verdict when detected: `Incorrect` if the information is publicly documented; otherwise leave the underlying topic as `Unsupported`.

## 6. Forced Pivot / 強制轉場

- Definition: The chatbot redirects to a different topic without answering the original question. The substituted topic may be accurate, but it is still a failure.
- Concrete example:
  Q: `Does this model support external display at 120Hz?`
  Bad A: `This product is designed with recycled materials and industry-leading energy efficiency.`
- Why it happens: The model detects weak retrieval coverage for the asked topic and falls back to safer marketing content that is easier to ground.
- Detection heuristic: Check whether the answer preserves the user's question. If the answer changes the topic, flag the pivot before checking the substituted content.
- Verdict when detected: `Incorrect` for the response behavior, even if the pivoted facts are true.

## 7. Equivocation / 偷換概念

- Definition: The chatbot answers using a different meaning of the same term than the one intended by the user. The answer looks responsive but operates on the wrong definition.
- Concrete example:
  Q: `What version does this device support?`
  Bad A: `It ships with the latest firmware version.` 
- Why it happens: Ambiguous terms in the prompt are resolved incorrectly by the model, and retrieval then reinforces the wrong sense of the word.
- Detection heuristic: Watch for overloaded terms like version, support, sharing, pairing, model, or compatibility. Confirm that the answer uses the same sense as the question.
- Verdict when detected: Usually `Incorrect`, because the response answers a different semantic question.

## 8. Red Herring / ESG Shield / 不相干謬誤 / 環保盾牌

- Definition: When the chatbot cannot answer, it fills space with accurate but irrelevant sustainability, environmental, privacy, or responsibility claims. This creates the appearance of substance without answering the question.
- Concrete example:
  Q: `What is the memory bandwidth of this chip?`
  Bad A: `The company is committed to privacy and carbon-neutral operations across its supply chain.`
- Why it happens: Marketing and policy pages are often highly retrievable, so the model substitutes well-grounded but irrelevant material for missing technical evidence.
- Detection heuristic: If the question is technical but the answer suddenly turns to ESG, privacy, or broad corporate values, treat that as a red-herring signal first.
- Verdict when detected: `Incorrect` for relevance failure, regardless of whether the filler claims themselves are true.

## 9. Over-Answering / 過度補充

- Definition: The chatbot pads the answer with historically true but currently irrelevant material, sometimes reintroducing facts that mislead or contradict the present-day answer. More text makes the answer worse, not better.
- Concrete example:
  Q: `Does the current service still require this legacy app?`
  Bad A: `Today it works in the browser, but historically users needed the legacy app, which was central to the old workflow and is still an important part of the platform story.`
- Why it happens: Retrieval returns archival and current sources together, and the model over-aggregates instead of prioritizing what is current and relevant.
- Detection heuristic: Look for long answers that add old generations, retired workflows, or previous policies after already giving a current answer.
- Verdict when detected: `Partly correct` if the current answer is still clear, or `Outdated` / `Incorrect` if the padding materially misleads the user.
