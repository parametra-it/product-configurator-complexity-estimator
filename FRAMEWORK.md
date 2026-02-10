# Product Configurator Assessment Framework by PARAMETRA

---

## Qualitative, Not Quantitative

This framework produces **qualitative assessments**, not numerical scores or price estimates.

Why? Because a number creates false precision. Saying "your project is a 7.3 out of 10" implies a level of certainty that doesn't exist at the scoping stage. A qualitative observation — *this dimension is demanding because of cascading dependencies across product families* — communicates the right information without pretending to be more precise than the analysis allows.

## Decision Graphs, Not Checklists

Complexity isn't a checklist you walk through sequentially. It's a branching landscape where each answer changes which questions matter next.

A kitchen configurator and a chair configurator don't just score differently on the same questions — they face *different questions entirely*. The kitchen manufacturer needs to explore compositional dependencies and spatial constraints. The chair manufacturer needs to explore material variety and visualization fidelity.

This framework uses **decision graphs** — branching question maps where each answer opens the next relevant question. The path through the graph *is* the assessment.

## The Four Factors Model

The framework evaluates four dimensions:

1. **[Catalog Structure](skill/factors/catalog-structure.md)** — The structure of your product range and the rules that govern configuration
2. **[Visualization](skill/factors/visualization.md)** — How products are visually represented, from no visualization to parametric 3D
3. **[Integrations](skill/factors/integrations.md)** — How deeply the configurator connects to your business systems
4. **[UX & Extras](skill/factors/ux.md)** — The interaction design, visual polish, device coverage, and testing depth

These four factors were identified through real-world project experience. They represent the dimensions that most consistently determine the effort, cost, and risk profile of a configurator project. The factors are designed to be **independently assessable** — you can evaluate your catalog structure without knowing anything about your integration needs.

## Factor Interactions and Compounding

While each factor can be assessed independently, they interact in practice. For example:

- High catalog complexity combined with frequent, deep integrations creates compounding challenges: the configuration rule engine must stay synchronized with external systems.
- Many 3D models combined with wide material variety demands careful performance optimization.
- Consumer-facing UX expectations combined with complex configuration flows require substantial design investment.
- Complex visualization combined with mobile device support creates aggressive optimization requirements.

Compounding also occurs **within** a single factor. When multiple complexity dimensions within the same factor are all elevated, they amplify each other. A catalog with cascading dependencies is one level of challenge; a catalog with cascading dependencies *and* cross-product shared elements *and* a high variant count is a materially harder problem — because each dimension multiplies the others' impact.

The framework acknowledges these interactions but doesn't try to model them mathematically. Understanding that they exist is what matters at the scoping stage.

## Complexity Is Not Linear

**Overall project complexity is not the sum of individual factor assessments.**

A project with significant complexity on one factor and low complexity on the other three is often more manageable than a project that's moderately complex across all four. Why? Because concentrated complexity has a clear focus, while distributed complexity creates a wider surface area of risk and coordination.

The framework gives you a complexity profile, not a mere total. When reading it, pay attention to the *shape* of the profile, not just the single levels.

### Conditional Ranges

Some assessments surface structural unknowns — questions that cannot be answered during the evaluation but that would materially change a factor's complexity level. In these cases, the profile expresses the factor as a conditional range (e.g., "Moderate → High") with the conditions that determine where in the range the project actually sits. This is more honest than collapsing uncertainty into a single label.

### Factor Priority

When two factors land at the same complexity level, not all factors carry equal weight. Catalog Structure — the engineering core — generally outweighs Integrations, which outweighs Visualization, which outweighs UX & Extras. This ordering serves as a tiebreaker for identifying the primary complexity driver, not as an absolute ranking.

## Build vs. Buy

The complexity profile is relevant whether you're considering custom development or an off-the-shelf platform. If your project is straightforward across all factors, a platform may serve you well. If you have concentrated complexity in catalog rules or integrations, custom development often provides better long-term value.

## Limitations

This framework is a scoping tool, not a specification. It helps you understand the *category* of challenge you're facing, not the precise technical requirements. It does not replace a detailed technical analysis, a proper requirements process, or prototyping work for novel features.

---

*This assessment framework is developed by [PARAMETRA](https://www.parametra.it), an Italian software house specialized in custom product configurators and integration layers for manufacturing companies. It reflects methodology used to scope real-world configurator projects across the furniture and nautical industries.*
