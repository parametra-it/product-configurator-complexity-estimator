# Factor 3: Integrations

**A configurator that lives in isolation is simple. A configurator that talks to your business systems is where real operational value — and real complexity — begins.**

---

## Overview

Many companies picture a product configurator as a standalone visual tool — the customer explores options, sees the result, and contacts the company. That's a valid starting point, and it's the simplest one.

But the real value of a configurator often comes from what happens *after* the user finishes configuring. Does the configuration become a quote? Does it flow into an ERP system? Does it generate a production order?

The depth of integration with existing business systems is what separates a marketing tool from an operational one. And notably, integrations affect **ongoing operational costs** (maintenance, monitoring, support) as much as initial development. A connection that works on day one still needs to work reliably on day 500.

This factor explores where your project sits on the integration spectrum.

---

## Decision Graph

### Q1: What happens when the user finishes configuring?

The output of the configuration determines the minimum integration requirement.

*Why this matters:* The output type defines the data structure and system connectivity needed. A simple inquiry needs minimal integration. A production-ready order needs the configurator to speak the language of your business systems.

- **The user sees the result and contacts you** (email, phone, contact form — the configuration is just a visual reference) → go to Q1a
- **The configurator captures a structured summary** (a saved configuration, a PDF, a shareable link — but no connection to business systems) → go to Q2
- **The configuration feeds into a business process** (quote generation, order creation, lead capture in CRM) → go to Q3

#### Q1a: No data capture

The configurator is purely visual. The user explores and then reaches out through standard channels. The configuration itself isn't formally captured or transmitted.

*Complexity signal:* This factor contributes **negligible complexity**. No integration work is needed. The configurator is self-contained.

*What to watch for:* This is the simplest starting point, but consider whether you're losing valuable data. Every configuration a user builds and abandons is a signal about what your market wants. Even minimal data capture (what was configured, even if anonymized) can be valuable.

→ **End of this branch.** Proceed to next factor.

---

### Q2: What form does the captured data take?

The configurator saves the configuration, but doesn't send it to external systems.

*Why this matters:* Even without external integrations, how the data is stored and presented affects development scope.

- **Summary display only** — the configuration is shown as a recap screen or list; nothing is saved persistently → go to Q2a
- **Saved configuration** — the user can save, retrieve, and share their configuration (link, account, or local storage) → go to Q2b
- **Document generation** — the configurator produces a PDF, specification sheet, or technical document → go to Q2c

#### Q2a: Summary display

A recap of selections shown at the end of the configuration flow. No persistence, no export.

*Complexity signal:* This suggests **low complexity** for integrations. The configurator just presents what it already knows. The development effort is in the UI presentation, not in data handling.

→ **End of this branch.** Proceed to next factor.

#### Q2b: Saved configurations

Users can save their configuration and come back to it, or share it with others via a link or code.

*Complexity signal:* This suggests **low-to-moderate complexity**. Saving configurations requires a persistence layer (database, cloud storage) and user identification or anonymous session management. Shareable links need a URL scheme and data serialization. It's not complex integration, but it's not trivial infrastructure either.

*What to watch for:* How long should saved configurations live? Days, months, forever? What happens when the catalog changes — does an old saved configuration still work if a material has been discontinued?

→ **End of this branch.** Proceed to next factor.

#### Q2c: Document generation

The configurator produces a document — typically a PDF — summarizing the configuration with images, specifications, and possibly preliminary pricing.

*Complexity signal:* This suggests **moderate complexity**. PDF generation with product images, formatted specifications, and branding requires a rendering pipeline. If the document includes pricing, you need a pricing engine. If it includes technical specifications or dimensioned drawings, the data model must be richer.

→ **End of this branch.** Proceed to next factor.

---

### Q3: Which business systems does the configurator connect to?

The configuration feeds into one or more business processes. Let's understand the landscape.

*Why this matters:* Each system connection is an integration project with its own API, data model, authentication, and error handling. The number and nature of connected systems is a primary complexity driver.

- **One system** (typically CRM, ERP, or e-commerce platform) → go to Q4
- **Two to three systems** (e.g., CRM for leads + ERP for orders, or e-commerce + ERP) → go to Q5
- **Many systems** (full operational integration: PIM, ERP, CRM, production management, logistics) → go to Q6

---

### Q4: Single-system integration

The configurator connects to one external system.

*Why this matters:* A single integration point is the most common and manageable scenario. The complexity depends on the depth of that connection.

- **One-way push** — the configurator sends data to the system (e.g., pushes a lead to CRM, or adds a configured product to an e-commerce cart) → go to Q4a
- **Bidirectional** — the configurator both reads from and writes to the system (e.g., reads pricing from ERP, writes back the order) → go to Q4b

#### Q4a: One-way single-system integration

The configurator pushes data to one system. The data flow is clear: configuration completes, data is packaged and sent.

*Complexity signal:* This suggests **moderate complexity** for integrations. The work involves API integration, data mapping (translating the configurator's data model to the target system's format), and error handling (what happens when the push fails?). It's well-understood engineering work with predictable scope.

*What to watch for:*
- Is the target system's API well-documented and stable?
- Who maintains the integration when the target system's API changes?
- What data validation is needed before pushing? (Garbage in, garbage out applies doubly to automated system integration.)

→ go to Q7

#### Q4b: Bidirectional single-system integration

The configurator reads from and writes to the same system. Example: reading product rules or pricing from an ERP during configuration, and writing back the completed order.

*Complexity signal:* This suggests **significant complexity**. Bidirectional data flow introduces challenges that don't exist in push-only scenarios: data freshness (is the pricing current?), conflict handling (what if the data changes during configuration?), and error recovery (what if the read succeeds but the write fails?).

*What to watch for:*
- How frequently does the source data change? Real-time pricing that updates hourly is different from a price list that changes quarterly.
- What's the acceptable latency? Can the configurator cache pricing data, or must every calculation hit the live system?

→ go to Q7

---

### Q5: Multi-system integration (2-3 systems)

The configurator connects to multiple business systems, each serving a different purpose.

*Why this matters:* Multiple integrations introduce coordination challenges. Data must be consistent across systems, operations may need to happen in sequence, and failures in one system shouldn't corrupt data in another.

- **Independent integrations** — each system receives its own data independently (e.g., CRM gets the lead, ERP gets the order — neither depends on the other) → go to Q5a
- **Orchestrated integrations** — the systems depend on each other in sequence (e.g., first check inventory in ERP, then create quote in CPQ, then push lead to CRM) → go to Q5b

#### Q5a: Independent multi-system integration

Multiple systems, but each integration is self-contained. They happen in parallel or independently.

*Complexity signal:* This suggests **significant complexity**. While each integration is independently manageable, the total scope is the sum of multiple integration projects, each with its own API, data model, and error handling. Testing and monitoring multiply accordingly.

→ go to Q7

#### Q5b: Orchestrated multi-system integration

Systems are connected in a workflow where one step depends on the previous. The configurator orchestrates a multi-step business process.

*Complexity signal:* This suggests **high complexity**. Orchestrated workflows need transaction-like behavior (what happens if step 2 fails after step 1 succeeded?), retry logic, state management, and often a message queue or workflow engine. The configurator becomes a business process orchestrator, not just a frontend tool.

*What to watch for:*
- What happens when one system is unavailable? Does the entire flow fail, or can it degrade gracefully?
- Who monitors the workflows? Failed orchestrations need visibility and intervention capability.

→ go to Q7

---

### Q6: Deeply integrated system

The configurator is woven into the company's operational fabric, connecting to multiple systems with complex data flows. It might pull product rules from a PIM, check real-time inventory, push orders to production management, sync customer data with CRM, and generate Bills of Materials.

*Complexity signal:* This suggests **very high complexity** for integrations. At this level, the configurator is itself a platform — a middleware layer that speaks to multiple systems and maintains data consistency across all of them. This requires dedicated integration architecture, monitoring infrastructure, and ongoing operational support.

*What to watch for:*
- Do all the target systems have stable, well-documented APIs?
- Is there existing middleware or an integration platform (ESB, iPaaS) that can be leveraged?
- Who owns the integration long-term? This level of connectivity needs ongoing engineering attention — it's not a build-and-forget effort.
- How are system upgrades handled? When the ERP updates its API, who ensures the configurator still works?

→ go to Q7

---

### Q7: Real-time requirements

Regardless of how many systems are involved, the timing of data exchange matters.

*Why this matters:* Real-time data dependencies dramatically increase both development complexity and operational sensitivity. A configurator that checks live inventory on every click behaves very differently from one that sends a batch overnight.

- **Asynchronous** — data is exchanged after the user finishes (batch processing, queued operations) → go to Q7a
- **Near real-time** — data should flow within seconds, but small delays are acceptable (e.g., checking stock availability when the user views a summary) → go to Q7b
- **Real-time** — the configurator depends on live data to function correctly during configuration (e.g., dynamic pricing, live inventory that affects available options) → go to Q7c

#### Q7a: Asynchronous data exchange

Data is sent after configuration is complete. Processing can happen in the background.

*Complexity signal:* Asynchronous integration is the simplest timing model. It decouples the user experience from system response times and reduces the impact of temporary system unavailability. **This adds minimal timing-related complexity.**

→ go to Q8

#### Q7b: Near real-time data exchange

Data flows quickly but doesn't need to be instantaneous. The user might see a brief loading state while data is fetched.

*Complexity signal:* This adds **moderate timing-related complexity**. The configurator needs loading states, timeout handling, and graceful degradation when responses are slow. Caching strategies become important — can you cache for 5 minutes? An hour?

→ go to Q8

#### Q7c: Real-time data dependency

The configurator requires live data to function. Configuration choices depend on current system state.

*Complexity signal:* This adds **significant timing-related complexity**. The configurator must handle latency, connection drops, data races (what if the data changes between the user's click and the server response?), and must provide a responsive experience despite depending on external systems. WebSocket connections, optimistic updates, or server-sent events may be needed.

*What to watch for:* Real-time dependencies create operational fragility. If the external system goes down, the configurator goes down — or at least degrades. Plan for this explicitly.

→ go to Q8

---

### Q8: User roles and permissions

One final integration dimension: does the configurator serve different types of users with different capabilities?

*Why this matters:* User roles add an authentication and authorization layer that affects both the integration architecture and the frontend logic.

- **Single user type** — everyone sees the same thing and has the same capabilities → go to Q8a
- **Multiple user types** — different users see different options, pricing, or capabilities (e.g., end consumer vs. dealer vs. internal sales) → go to Q8b

#### Q8a: Single user type

All users have the same experience. No authentication or role-based logic needed.

*Complexity signal:* This adds **no additional complexity** from a permissions standpoint.

→ **End of this branch.** Proceed to next factor.

#### Q8b: Multiple user types

Different users see different versions of the configurator — different pricing, different product access, different capabilities.

*Complexity signal:* This adds **moderate-to-significant complexity**. Role-based access requires authentication (login system or SSO integration), authorization logic (what each role can see and do), and potentially different data feeds per role. The testing surface also multiplies — every feature must be verified for each user type.

*What to watch for:*
- How many distinct roles are there? Two roles (consumer vs. dealer) is manageable. Five roles with nuanced permission differences is substantially more complex.
- Where do user accounts live? If there's an existing identity system (Active Directory, SSO, e-commerce accounts), integration with that system is its own project.

→ **End of this branch.** Proceed to next factor.
