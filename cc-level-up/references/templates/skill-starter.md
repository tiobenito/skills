# Skill Starter Template

## Questions to Ask First

1. "What task do you want to speed up?" (identifies the core action)
2. "Walk me through how you do it now" (captures the current workflow)
3. "What does a good output look like?" (defines success)

## Template

```markdown
---
name: [skill-name]
description: >
  [What it does in 1-2 sentences. Include trigger phrases like "use when
  the user says X, Y, or Z".]
---

# [Skill Name]

[1-2 sentence overview of what this skill does and when to use it.]

## Steps

1. [First thing Claude should do — e.g., "Ask for the raw data or paste"]
2. [Processing step — e.g., "Extract key fields: date, amount, status"]
3. [Output step — e.g., "Format as a clean table with headers"]

## Output Format

[Describe or show the expected output shape — e.g., a table, bullet list, or template]

## Rules

- [Any constraint — e.g., "Never round dollar amounts"]
- [Any preference — e.g., "Use bullet points, not paragraphs"]
```

## Tips for the Skill

- Name should be lowercase-with-dashes, 2-4 words
- Description must include trigger phrases so Claude knows when to use it
- Keep skills focused — one task per skill
- Include an output format section so the user knows what to expect
- Don't over-engineer the first version — they can refine it later
