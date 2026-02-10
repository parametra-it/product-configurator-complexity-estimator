# Using the AI-Assisted Complexity Assessment

This repository includes an AI skill (`skill/SKILL.md`) that turns the complexity framework into an interactive, guided conversation. Instead of reading through the decision graphs yourself, an AI assistant navigates the relevant questions based on your answers and produces a structured complexity profile.

---

## Format

The skill follows the [Claude Code skill convention] a YAML frontmatter header with metadata, followed by a markdown system prompt with knowledge files in a `factors/` subdirectory. Skills can be invoked in Claude.ai, Claude Code, and Claude Cowork using custom slash-command or natural language such as:

> "I'm considering building a configurator for my furniture line. Can you help me assess the complexity?"

Please refer to official Claude documentation to install it.

This format can be adapted to other AI tool conventions: ChatGPT Custom GPTs, OpenAI Assistants, or any LLM that supports system prompts and knowledge files.



## What to Expect

A typical assessment takes **10-15 minutes**. The AI navigates decision graphs across four complexity factors, asking branching questions adapted to your specific situation:

- **Catalog Structure** — how your products relate and what rules govern configuration
- **Visualization** — what kind of visual representation you need (from none to parametric 3D)
- **Integrations** — how the configurator connects to your business systems
- **UX & Extras** — interaction design, device support, and quality assurance

At the end, you receive a complexity profile: a qualitative assessment per factor, overall complexity observations, and practical next steps.

The assessment is a **scoping tool** — it helps you understand the category of challenge you're facing, not a specification or quote. Use it as a starting point for informed conversations with development partners.

---

*This AI skill is part of the [Product Configurator Complexity Estimator](README.md), developed by [PARAMETRA](https://www.parametra.it) — an Italian software house specialized in custom product configurators for manufacturing companies.*
