# Example: Attribute Mismatch

## Scenario

A user asks a customer service chatbot about the material used in a laptop's construction.

## User Question

> What material is the laptop case made of?

## Chatbot Response

> The laptop comes in Space Black and Silver color options, featuring a stunning new dark appearance that is anodized to create a breakthrough chemistry that etches into the surface.

## Skill Output

**Response type:** Direct answer

**Overall verdict:** Incorrect (Attribute Mismatch)

### Claim-by-claim findings

1. **Claim:** The laptop is available in Space Black and Silver.
   - **Verdict:** Correct
   - **Why:** Color options are publicly documented on the manufacturer's product page.

2. **Claim:** The dark appearance uses anodized etching chemistry.
   - **Verdict:** Correct
   - **Why:** This finishing process is described in official product materials.

3. **Claim:** This answers the user's question about case material.
   - **Verdict:** Incorrect
   - **Why:** The user asked about *material* (e.g., aluminum, titanium, polycarbonate). The chatbot answered about *color and finish*. The facts stated are individually true, but they address the wrong attribute.

### Error pattern detected

**Attribute Mismatch (屬性錯位)** — The chatbot correctly identified the product but answered a different attribute than what was asked. Retrieval found appearance-related content and the model did not distinguish "material" from "color/finish" as separate attributes.

### Bottom line

The response contains accurate facts about color options and finishing processes, but completely fails to answer the actual question. The case material (recycled aluminum) was never mentioned. This is a relevance failure, not a factual error.
