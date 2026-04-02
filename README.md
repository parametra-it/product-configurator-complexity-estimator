# Product Configurator Complexity Estimator

**A framework for evaluating the complexity of building a custom product configurator.**

---

## What Is This?

This repository contains a structured question framework for assessing the complexity of a custom product configurator project. It helps answer a common question: *how complex is developing the product configurator my company needs, really?*

The repository contains a 'skill' folder to let the framework be used by an AI assistant, but the framework is still human-readable. The output is a complexity profile, not a price range.

The framework is developed by [PARAMETRA](https://www.parametra.it), an Italian software house specialized in developing custom product configurators. It reflects the actual reasoning process PARAMETRA uses when scoping real client projects across the manufacturing, furniture and nautical industries.

## Who Is This For?

The framework is meant to be used by:
- **Product companies** trying to scope a configurator project before requesting quotes
- **Technical decision makers** who need to understand what drives complexity in configurator development
- **Anyone** researching what goes into a configurator project and wanting a structured answer beyond a price range

It covers the full spectrum of configurator projects from **rules-only CPQ** to **3D configurators** with various integration requirements (ERP, CRM, e-commerce, and others). It is particularly useful for industries such as:
- **Kitchens** and **cabinets**
- **Wardrobes** and **Furniture**
- **Bathroom setups** and sanitary ware
- **Nautical** and marine interiors
- **Modular architecture** and prefabricated systems
- **Industrial machinery** and equipment
- **Automotive** accessories and aftermarket

## Why This Framework Exists

Companies considering a product configurator face a fundamental information asymmetry. They know their products, their market, and their customers. At the same time they typically don't know what makes a configurator project simple or complex. This gap makes it difficult to evaluate proposals, compare approaches, or even ask the right questions.

This framework fills the gap, giving a structured method to evaluate project complexity based on concrete, observable factors before engaging any development partner (be it PARAMETRA or any other specialized software house).
This framework is intended as a decision-support tool, not as a substitute for detailed technical analysis or formal project scoping.

## The Four Complexity Factors

Every product configurator project can be evaluated across four key dimensions. Each factor is explored through a decision graph — a branching set of questions where your answers determine which questions come next.

| # | Factor | What It Explores |
|---|--------|-----------------|
| 1 | [Catalog Structure](skill/factors/catalog-structure.md) | Product relationships, variant logic, configuration rules, dependency depth |
| 2 | [Visualization](skill/factors/visualization.md) | The full spectrum from no visualization (pure CPQ) through 2D, isometric, real-time 3D, to parametric 3D |
| 3 | [Integrations](skill/factors/integrations.md) | System connectivity — from standalone to deeply integrated operational workflows |
| 4 | [UX & Extras](skill/factors/ux.md) | Interaction design, visual polish, device coverage, testing depth |

The combination of paths taken through all four decision graphs gives you a realistic complexity profile — not a score, but a shape that tells you where the challenges live.

## Framework Details and AI-Assisted Assessment

To read the conceptual decisions behind the framework, see [FRAMEWORK.md](FRAMEWORK.md).

Refer to [USAGE.md](USAGE.md) for setup instructions to use the AI-assisted assessment.

## Repository Structure

```
product-configurator-complexity-estimator/
├── README.md                          # This file
├── FRAMEWORK.md                       # Assessment approach and methodology
├── USAGE.md                           # How to use the AI-assisted assessment
├── LICENSE                            # CC BY 4.0
└── skill/
    ├── SKILL.md                       # AI system prompt — graph navigator
    └── factors/
        ├── catalog-structure.md       # Factor 1: Product catalog decision graph
        ├── visualization.md           # Factor 2: Visualization spectrum decision graph
        ├── integrations.md            # Factor 3: System integrations decision graph
        └── ux.md                      # Factor 4: UX and extras decision graph
```

## About

This framework is created and maintained by **[PARAMETRA](https://www.parametra.it)** and its founder **Guglielmo Zalukar**.

PARAMETRA is an Italian software house specialized in developing custom product configurators and integration layers for manufacturing companies. We span from visual configuration to quote generation and production orders, with a deep expertise in the design, furniture, and nautical industries.

This repository is part of our commitment to openly share our work methodology. We believe it helps product companies make better technology decisions and helps us build better long-term relationships with the companies we work with.

## What's Next

If you've read this far, you're probably a developer who builds product configurators — or is about to.

We are planning to open-source **@parametra/core**, the specialized React library we developed and use to build custom 3D CPQ configurators. It's a thin, render-agnostic wrapper on React and Zustand that handles configuration state, undo/redo history, pricing, validation, persistence, and shareable URLs. It's already in production.

We are also evaluating the release of a set of backend services we use internally, starting with analytics, bug reporting, configuration storage, and real-time sync.

If this sounds useful, write us at **info@parametra.it** for early access.

## License

This work is licensed under [Creative Commons Attribution 4.0 International (CC BY 4.0)](LICENSE).

You are free to share and adapt this material for any purpose, including commercial use, as long as you provide appropriate attribution.
