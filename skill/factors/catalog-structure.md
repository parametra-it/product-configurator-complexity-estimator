# Factor 1: Catalog Structure

**The structure of your product catalog — how products relate to each other, how choices interact, and how rules propagate — is the single biggest driver of configurator complexity.**

---

## Overview

A configurator for a collection of chairs with independent color options is a fundamentally different project than a configurator for custom kitchens where every choice constrains the next. Before anything else — before visualization, before integrations, before UX polish — the shape of your catalog determines the engineering challenge.

This factor explores your catalog through a series of branching questions. Each answer narrows the picture and opens the next relevant question.

---

## Decision Graph

### Q1: What does the user configure?

This is the starting point. The answer determines the entire downstream path.

*Why this matters:* A configurator that handles a single product at a time is architecturally simpler than one that manages relationships between multiple products. This distinction alone can shift a project from straightforward to demanding.

- **A single product** (a chair, a table, a lamp) → go to Q2
- **A product with accessories** (a sofa with cushions, a desk with cable management) → go to Q3
- **A composition of products** that must work together (a modular shelving system, a bathroom setup) → go to Q5
- **A system with dependent components** (a kitchen with base cabinets, wall cabinets, countertops, appliances) → go to Q6

---

### Q2: How do variants work for this product?

You're configuring a single product. Now let's understand the option space.

*Why this matters:* The number of variant dimensions and their independence from each other determines how the configurator's data model and UI need to be structured.

- **Few independent options** (3-5 choices like color, size, finish — each chosen freely) → go to Q2a
- **Many independent options** (10+ variant dimensions, but still freely combinable) → go to Q2b
- **Options with dependencies** (choosing one thing restricts what you can choose next) → go to Q4

#### Q2a: Simple product, few independent options

A product with a handful of freely combinable choices. This is the most straightforward configurator scenario.

*Complexity signal:* This suggests **low complexity** in catalog structure. The configurator is essentially a visual product selector — the engineering challenge is minimal on the catalog side. Effort will be driven by other factors (visualization, integrations, UX).

*What to watch for:* Even simple catalogs can grow. If you expect to add many more products or option dimensions over time, consider whether the data architecture should anticipate that growth.

→ **End of this branch.** Proceed to next factor.

#### Q2b: Single product, many independent options

A product with a wide option space but no constraints between choices. Think of a product with dozens of material/color/accessory combinations that are all valid.

*Complexity signal:* This suggests **low-to-moderate complexity** in catalog structure. The logic is simple (no rules to enforce), but the data volume is significant. The configurator needs clean data architecture and a UI that doesn't overwhelm the user with choices.

*What to watch for:* Large option counts put pressure on the UI/UX design. A flat list of 50 materials is unusable — you'll need intelligent grouping, search, or filtering. The complexity migrates from the rule engine to the interface design.

→ **End of this branch.** Proceed to next factor.

---

### Q3: How do the accessories relate to the main product?

You're configuring a product with accessories or add-ons. Let's understand the relationship.

*Why this matters:* Accessories can be purely optional extras, or they can interact with the main product in ways that require validation logic.

- **Accessories are independent** — any accessory works with any configuration of the main product → go to Q3a
- **Accessories depend on the main product** — which accessories are available changes based on the main product's configuration → go to Q4
- **Accessories interact with each other** — choosing one accessory affects what other accessories are available → go to Q4

#### Q3a: Independent accessories

The main product and its accessories are configured separately. Any accessory works with any product configuration.

*Complexity signal:* This suggests **low-to-moderate complexity**. The configurator handles the main product and a shopping-list of extras. No rule engine needed — just a clean way to present the options and build a complete order.

→ **End of this branch.** Proceed to next factor.

---

### Q4: How deep do the dependencies go?

You've indicated that choices in your configurator affect other choices. Let's understand the depth.

*Why this matters:* Dependencies (also called configuration rules or constraints) are where complexity escalates sharply. A configurator without dependencies is essentially a product filter. A configurator with deep dependency chains is a rule engine that must validate every combination and guide the user through a constrained decision tree.

- **One level of dependency** — choosing A filters the options for B, but B doesn't affect anything else → go to Q4a
- **Cascading dependencies** — choices propagate through multiple levels (A → B → C → D) → go to Q4b
- **Bidirectional dependencies** — changing a downstream choice can force upstream recalculation → go to Q4c

#### Q4a: Single-level dependencies

One choice filters another, but the chain stops there. Example: selecting a wood type restricts available finishes, but the finish doesn't constrain anything further.

*Complexity signal:* This suggests **moderate complexity**. The configurator needs a rule engine, but a relatively simple one — essentially a set of lookup tables. The key challenge is maintaining those rules as the catalog evolves.

*What to watch for:* How are the rules maintained? If a product manager can update them in a spreadsheet or admin panel, ongoing maintenance is manageable. If every rule change requires developer intervention, this becomes a bottleneck.

→ **End of this branch.** Proceed to next factor.

#### Q4b: Cascading dependencies

Choices propagate through multiple levels. Example: base cabinet size → determines valid countertop dimensions → determines valid sink options → determines valid faucet positions.

*Complexity signal:* This suggests **significant complexity**. The configurator needs a proper rule engine that can evaluate chains of constraints and maintain consistency across levels. User experience design becomes critical — the user needs to understand *why* an option disappeared and how to get it back.

*What to watch for:*
- How many levels deep do the cascades go? Two levels is manageable; four or five levels is architecturally demanding.
- Can the rules be expressed declaratively (in data), or do they require procedural logic (code)?
- What happens when a user backtracks? Does changing an early choice invalidate downstream selections?

→ go to Q7

#### Q4c: Bidirectional dependencies

Changing a downstream choice forces recalculation upstream. Example: selecting a specific sink requires adjusting the cabinet width, which may affect adjacent cabinets.

*Complexity signal:* This suggests **high complexity**. Bidirectional dependencies create the possibility of circular constraints and require a constraint solver rather than simple cascade logic. The configurator must find a valid state after each change, which is a fundamentally harder engineering problem.

*What to watch for:* Bidirectional dependencies are rare in simpler products. If you're seeing them, you're likely dealing with a compositional or system-level configurator. Make sure the rules can actually be resolved deterministically — some constraint combinations may have no valid solution.

→ go to Q7

---

### Q5: How do the products in the composition relate?

You're configuring a composition — multiple products that must work together. Let's understand the relationships.

*Why this matters:* Compositions introduce cross-product constraints. The configurator doesn't just validate each product in isolation — it must ensure the whole assembly is coherent.

- **Products are placed independently** — they're sold together but don't constrain each other (e.g., a furniture set where each piece is configured separately) → go to Q5a
- **Products share constraints** — choices on one product affect what's available on another (e.g., matching materials across a set, or components that must fit together dimensionally) → go to Q5b
- **Products are physically dependent** — components must fit together with exact tolerances, and changing one can require changes to others (e.g., modular systems, built-in furniture) → go to Q6

#### Q5a: Independent products in a composition

The products are sold as a set but configured independently. Example: a bedroom set where the bed, nightstands, and dresser are each configured on their own.

*Complexity signal:* This suggests **moderate complexity**. The challenge is primarily in the user interface — presenting a multi-product configuration flow clearly — rather than in the rule engine. Some shared constraints (like matching available materials across the set) may apply.

→ **End of this branch.** Proceed to next factor.

#### Q5b: Constrained composition

Products in the composition share constraints. Example: a modular sofa system where choosing a corner module determines which straight modules can connect to it.

*Complexity signal:* This suggests **significant complexity**. The configurator needs cross-product validation — a rule engine that understands not just individual product constraints but the relationships between products. Data modeling becomes critical: how do you represent "this module can connect to that module but not that one"?

→ go to Q7

---

### Q6: Describe the system complexity

You're dealing with a system of dependent components — the most complex category of product configuration.

*Why this matters:* System-level configurators (kitchens, walk-in closets, modular office systems) require the configurator to act as an engineering tool, not just a visual selector. Every component exists in relation to others, and the total system must be valid as a whole.

- **Fixed system templates** — the user selects from predefined layouts and configures within that structure → go to Q6a
- **Flexible system assembly** — the user builds the system freely from available components, with constraint enforcement → go to Q6b

#### Q6a: Template-based system configuration

The user selects a layout template (e.g., L-shaped kitchen, U-shaped closet) and then configures each section within that structure. The overall shape is predetermined.

*Complexity signal:* This suggests **significant-to-high complexity**. Template-based approaches are a smart way to contain complexity — they eliminate free-form spatial planning while still allowing deep configuration within each section. The challenge is in the section-level dependencies and ensuring the templates cover real customer needs.

*What to watch for:* How many templates do you need? Each template is essentially a separate configuration flow. If the answer is "5 templates" that's manageable. If it's "we need to support any possible layout" — you're actually describing free-form assembly (Q6b).

→ go to Q7

#### Q6b: Free-form system assembly

The user assembles the system from available components without predefined templates. Components must fit together, and the system enforces rules about valid combinations, dimensions, and connections.

*Complexity signal:* This suggests **high-to-very-high complexity**. This is the most demanding category of catalog structure. The configurator becomes a constrained design tool — it needs spatial awareness, connection logic, dimension validation, and a sophisticated rule engine that can evaluate the entire system after every change.

*What to watch for:*
- Can the rules be represented as data, or do they require custom code per product family?
- How do you handle invalid states? Can the system always find a valid resolution, or might the user reach a dead end?
- Is there a practical limit on system size (number of components)? Performance and usability both degrade with very large systems.

→ go to Q7

---

### Q7: How will the catalog evolve?

You've described a configurator with meaningful complexity. One final question: how will this grow over time?

*Why this matters:* A configurator with complex rules is a living system. If the catalog changes frequently, the rule maintenance burden becomes a significant ongoing concern — sometimes exceeding the initial development effort.

- **Catalog is stable** — product range and rules change rarely (once or twice a year) → go to Q7a
- **Catalog evolves regularly** — new products, new options, or updated rules on a monthly or quarterly basis → go to Q7b
- **Catalog is highly dynamic** — frequent changes, seasonal collections, or rapid product development cycles → go to Q7c

#### Q7a: Stable catalog

The product range and configuration rules are relatively fixed. Updates are infrequent.

*Complexity signal:* A stable catalog means the initial rule engineering is the main investment. Ongoing maintenance is minimal. This is the most favorable scenario for complex configurators — you do the hard work once.

→ **End of this branch.** Proceed to next factor.

#### Q7b: Regular evolution

New products and rule changes happen on a regular cadence.

*Complexity signal:* This adds a meaningful operational dimension. The configurator needs tooling for rule management — ideally a way for product managers (not developers) to add products and update constraints. Without this, every catalog update becomes a development project.

*What to watch for:* How are configuration rules authored today? If they live in spreadsheets or a PIM, there may be a path to automated rule import. If they live in people's heads, formalizing them is itself a significant effort.

→ go to Q8

#### Q7c: Highly dynamic catalog

The product range changes frequently. New options appear regularly, seasonal variations rotate, and the configuration rules must keep pace.

*Complexity signal:* This significantly amplifies overall complexity. A highly dynamic catalog requires a fully externalized rule engine — configuration logic that lives in data (not code) and can be updated without deployments. It also requires rigorous testing automation, because every catalog change can introduce unexpected interactions.

*What to watch for:* This is where the distinction between "building a configurator" and "building a configurator platform" becomes real. If the catalog changes weekly, you may need to invest as much in the content management and rule authoring tools as in the configurator itself.

→ go to Q8

---

### Q8: How large is the catalog?

You've described a configurator with both meaningful rule complexity and an evolving catalog. One more thing to understand: the scale of what you're managing.

*Why this matters:* Catalog size acts as a **multiplier** on everything discussed so far. The same dependency logic that's manageable with 20 products becomes a serious authoring, testing, and maintenance challenge at 200 or 2,000. This isn't about drawing a line at a specific number — it's about understanding whether scale amplifies the complexity you've already described.

*This question is for context, not classification.* Roughly how many distinct configurable products (or product families) are we talking about?

- **Small scale** — a handful of products or families (under ~20). Each product may be complex, but the total volume of rules and combinations stays human-manageable → go to Q8a
- **Medium scale** — dozens to low hundreds of products or families. Rule authoring and testing start requiring systematic processes → go to Q8b
- **Large scale** — hundreds or thousands of products or families. The catalog itself becomes a data management challenge → go to Q8c

#### Q8a: Small-scale evolving catalog

A limited number of products with evolving rules. The complexity lives in the depth of each product's configuration, not in the breadth of the catalog.

*Complexity signal:* Scale does not significantly amplify the complexity identified earlier. Rule management and testing can be handled with relatively lightweight processes. The focus should be on getting the rule engine architecture right — the volume won't overwhelm it.

→ **End of this branch.** Proceed to next factor.

#### Q8b: Medium-scale evolving catalog

Dozens to low hundreds of configurable products with evolving rules. This is where the interaction between catalog breadth and rule complexity starts to compound.

*Complexity signal:* Scale is a meaningful amplifier. At this volume, rule authoring becomes a process that needs its own tooling — product managers need structured ways to define, test, and deploy configuration rules. Manual approaches (spreadsheets, developer-mediated updates) start breaking down. Regression testing after catalog updates becomes non-trivial.

*What to watch for:* The real question is how similar the products are. If 100 products share a common configuration logic with minor variations, template-based rule authoring can contain the effort. If each product has unique rule structures, the authoring burden grows linearly with catalog size.

→ **End of this branch.** Proceed to next factor.

#### Q8c: Large-scale evolving catalog

Hundreds or thousands of configurable products with frequent changes. The catalog is a data asset in its own right.

*Complexity signal:* Scale is a **major amplifier**. At this volume, the catalog management infrastructure — rule authoring tools, validation pipelines, automated testing, staging environments — becomes as significant as the configurator itself. Every catalog update is a potential source of broken configurations, and the sheer number of possible interactions makes exhaustive manual testing impossible.

*What to watch for:* Projects at this scale almost always require a dedicated catalog management workflow separate from the configurator's development. The question shifts from "can the configurator handle this?" to "can the organization maintain this catalog reliably over time?"

→ **End of this branch.** Proceed to next factor.
