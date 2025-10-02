## Core Concept/Principle:
Treat AI prompts like structured code rather than conversational text. Use XML tags, markdown formatting, and clear sectional organization (role definition, task description, step-by-step plan, output format) to leverage how models are trained on structured data.

## Context/Example: 
Instead of "Please help me write a test plan," use:
```xml
<role>Expert QA Engineer</role>
<task>Create comprehensive test plan</task>
<requirements>
- Coverage for API endpoints
- Performance benchmarks
- Edge case scenarios
</requirements>
<output_format>Markdown with sections</output_format>
```

## Reference: 
https://www.youtube.com/watch?v=DL82mGde6wo&ab_channel=YCombinator
Claude conversation on AI prompting best practices, 2025-06-07

## Connections:
- **Similar**: [[Software Architecture Patterns]], [[API Design Principles]]
- **Opposite**: [[Conversational Prompting]]
- **Builds on**: [[Structured Communication]]
- **Enables**: [[Reliable AI Workflows]], [[Prompt Engineering Systems]]

---
Created: 2025-06-07 Tags: #systems-thinking #ai-prompting #structured-communication