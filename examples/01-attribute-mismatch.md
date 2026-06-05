# Example: Attribute Mismatch

## Scenario

A user asks a customer service chatbot about the material used in a laptop's construction.

## User Question

> What material is the laptop case made of?

## Chatbot Response

> The laptop is available in black and silver color options, with a matte finish that gives it a premium appearance.

## Skill Output

**Response type:** Direct answer

**Overall verdict:** Incorrect (Attribute Mismatch)

### Claim-by-claim findings

1. **Claim:** The laptop is available in black and silver.
   - **Verdict:** Correct
   - **Why:** Color options are publicly documented on the manufacturer's product page.

2. **Claim:** The finish is matte and gives a premium appearance.
   - **Verdict:** Correct
   - **Why:** The finish is described in official product materials.

3. **Claim:** This answers the user's question about case material.
   - **Verdict:** Incorrect
   - **Why:** The user asked about *material* (e.g., aluminum, magnesium alloy, polycarbonate). The chatbot answered about *color and finish*. The facts stated are individually true, but they address the wrong attribute.

### Error pattern detected

**Attribute Mismatch (屬性錯位)** — The chatbot correctly identified the product but answered a different attribute than what was asked. Retrieval found appearance-related content and the model did not distinguish "material" from "color/finish" as separate attributes.

### Bottom line

The response contains accurate facts about color options and finish, but completely fails to answer the actual question. The case material was never mentioned. This is a relevance failure, not a factual error.
