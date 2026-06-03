# Source Priority And Verdict Guide

## Source Order

Use the highest reliable tier available for the claim:

1. Official or primary source
- vendor product pages
- vendor support articles
- developer documentation
- standards/specification documents
- research papers or official datasets

2. High-quality secondary source
- reputable testing lab
- established technical publication
- direct transcript or archive of a primary source

3. Community evidence
- issue trackers
- forum threads
- user reports

Use tier 3 only when the claim is about real-world behavior not documented officially, and label it as anecdotal or community-reported.

## Verdict Shortcuts

- `Correct`: directly supported by the cited source
- `Partly correct`: contains both supported and unsupported elements
- `Unsupported`: no adequate evidence found
- `Outdated`: may once have been true, but current evidence contradicts it
- `Incorrect`: contradicted by reliable current evidence

## Negative Verdict Boundary

- Never mark a claim `Incorrect` or `Outdated` from internal knowledge alone.
- If internal knowledge conflicts with the claim, treat that as a search trigger, not a verdict.
- Use `Incorrect` or `Outdated` only when search finds a reliable contradicting source.
- If search finds no reliable support and no reliable contradiction, use `Unsupported` instead of a negative verdict.

## Distinguishing Fact From Inference

Use this wording pattern when needed:

- `The specification is confirmed, but the claimed outcome is an inference rather than an official statement.`
- `This source confirms X, but does not support Y.`
- `The conclusion may be reasonable, but I could not verify it as an official or directly tested claim.`

## Compact Output Template

```md
Overall verdict: Partly correct. The specs are supported, but the performance conclusion is not directly confirmed.

1. Claim: ...
- Verdict: Correct / Partly correct / Unsupported / Outdated / Incorrect
- Why: ...
- Source: [Title](https://example.com)

2. Claim: ...
- Verdict: ...
- Why: ...
- Source: [Title](https://example.com)

Bottom line: The answer mixes confirmed facts with inference. The weakest part is ...
```

## Chinese Output Template

```md
整體結論：這段內容屬於「部分正確」。其中硬體規格可被證實，但後段結論屬於推論，沒有直接來源支持。

1. 主張：...
- 查核結果：正確 / 部分正確 / 無法證實 / 資訊過時 / 錯誤
- 說明：...
- 來源連結：[標題](https://example.com)

2. 主張：...
- 查核結果：...
- 說明：...
- 來源連結：[標題](https://example.com)

結論整理：這段回答混合了可證實事實與未經證實推論，最需要小心的是 ...
```

## Preferred Chinese Phrasing

- `這一點可由官方資料直接證實。`
- `官方資料只證實前半句，沒有直接支持後半句結論。`
- `這比較像合理推測，不是官方明確說法。`
- `目前找不到足夠的一手來源支持這個說法。`
- `這個說法與目前可查到的官方資訊不符，屬於過時或錯誤資訊。`

## Common Failure Pattern

Watch for this structure:

1. accurate specs, feature details, or policy wording
2. a jump to user-experience or performance certainty
3. no direct evidence for the conclusion

When this pattern appears, explicitly say that the response uses true facts to lend credibility to an unverified conclusion.
