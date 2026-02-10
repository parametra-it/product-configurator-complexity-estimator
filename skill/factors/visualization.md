# Factor 2: Visualization

**How your products are visually represented — from no visualization at all to fully parametric 3D — determines an entire dimension of development effort.**

---

## Overview

Not every configurator needs 3D. Some don't need visualization at all. A CPQ (Configure, Price, Quote) system that lets a sales rep select options from a structured form is a valid configurator — it just doesn't render anything.

The visualization spectrum runs from pure logic (no visuals) through 2D image compositing, isometric pre-rendered views, real-time 3D, all the way to parametric 3D where geometry is generated from code. Where your project sits on this spectrum — and what's needed within that level — determines the complexity of this factor.

This decision graph helps determine where you are and what that means.

---

## Decision Graph

### Q1: Does the configurator need to show the product visually?

Start here. Not every configurator is visual.

*Why this matters:* Visualization is a major development dimension. If the configurator is purely functional (selecting options, generating quotes), this entire factor may not apply — and that's fine.

- **No — it's a pure CPQ or logic-based configurator** (option selection, pricing, quote generation — no product rendering) → go to Q1a
- **Yes — the user needs to see the product as they configure it** → go to Q2

#### Q1a: No visualization needed

The configurator is a structured decision tool without visual product rendering. The user selects options through forms, dropdowns, or guided flows, and the output is a specification, quote, or order.

*Complexity signal:* This factor contributes **negligible complexity**. The configurator's challenges live entirely in other factors (catalog structure, integrations, UX flow). This is common for B2B configurators where the buyer knows the product and needs accurate specification, not a visual preview.

→ **End of this branch.** Proceed to next factor.

---

### Q2: What type of visual representation do you need?

You need visualization. Let's determine what kind.

*Why this matters:* The visualization approach shapes the technology stack, the asset pipeline, and the performance characteristics. Each approach has different strengths, limitations, and cost profiles.

- **Static images** — pre-shot photos or renders that swap based on selections → go to Q3
- **2D compositing** — layered images assembled dynamically (e.g., a front-view of a sofa where the fabric texture is overlaid) → go to Q4
- **Isometric or pre-rendered 3D** — fixed-angle 3D renders, pre-generated for each configuration or key configurations → go to Q5
- **Real-time 3D** — interactive 3D scene the user can rotate, zoom, and explore → go to Q6

---

### Q3: Static image approach

The configurator shows pre-made images (photographs or renders) that correspond to the user's selections. When the user picks "oak finish," the image swaps to show the oak version.

*Why this matters:* This is the simplest visual approach, but it has a hard scaling limit. The number of images you need grows with the number of option combinations.

- **Few combinations** (under ~50 total visual states) → go to Q3a
- **Many combinations** (50+ visual states, or combinations that are impractical to pre-shoot/render) → go to Q3b

#### Q3a: Manageable static image set

A limited number of option combinations makes pre-shot or pre-rendered images practical. Each combination has its own image.

*Complexity signal:* This suggests **low complexity** for visualization. The main effort is in producing the images themselves (photography or rendering), not in the configurator technology. The configurator simply swaps images based on selections.

*What to watch for:* This approach becomes unmanageable as the option space grows. If the catalog will expand significantly, consider whether you'll eventually need a more dynamic approach.

→ **End of this branch.** Proceed to next factor.

#### Q3b: Too many combinations for static images

The option space is too large to practically pre-produce images for every combination. You'll need a more dynamic approach.

*This suggests moving to 2D compositing, isometric pre-renders, or real-time 3D. Let's explore which fits.*

→ return to Q2 and consider 2D compositing (Q4) or real-time 3D (Q6)

---

### Q4: 2D compositing approach

The configurator assembles a product image from layers — a base shape, overlaid textures, swappable components — all in 2D. The result looks like a single image but is dynamically composed.

*Why this matters:* 2D compositing avoids the complexity of 3D rendering while handling more combinations than static images. It works well for products with a natural "flat" or front-facing view — textiles, wall panels, simple furniture front-views, flooring.

- **Simple layering** — a few layers (base + texture overlay + optional accessory) → go to Q4a
- **Complex layering** — many layers with precise alignment, transparency, shadows, and component swaps → go to Q4b

#### Q4a: Simple 2D compositing

A small number of layers combined to show the configured product. Example: a cushion configurator that overlays the selected fabric texture onto a base shape.

*Complexity signal:* This suggests **low-to-moderate complexity** for visualization. The technology is straightforward (canvas or CSS compositing), but the asset pipeline matters — each layer needs to be produced with consistent dimensions, lighting, and alignment.

→ **End of this branch.** Proceed to next factor.

#### Q4b: Complex 2D compositing

Many layers with precise positioning, shadow integration, and component-level swaps. Example: a wardrobe front-view where doors, handles, internal dividers, and finishes are all separate layers that must align pixel-perfectly.

*Complexity signal:* This suggests **moderate complexity**. The compositing engine needs careful layer management, and the asset production pipeline becomes significant — every component needs to be rendered/photographed from the exact same angle, with consistent lighting and scale. Quality control across dozens of layers is non-trivial.

*What to watch for:* Complex 2D compositing can approach the effort of simple 3D rendering while being less flexible. If you find yourself needing multiple viewing angles or free rotation, the cost-benefit may tip toward real-time 3D.

→ **End of this branch.** Proceed to next factor.

---

### Q5: Isometric or pre-rendered 3D approach

The configurator shows the product from fixed 3D angles using pre-rendered images. The renders look three-dimensional, but the user can't freely rotate the view — they see the product from one or a few predetermined perspectives.

*Why this matters:* This is a middle ground between 2D and real-time 3D. It looks good and avoids the performance challenges of real-time rendering, but it shares the scaling problem of static images — you need a render for every angle × every configuration.

- **Single fixed angle** with configuration-dependent renders → go to Q5a
- **Multiple angles** (e.g., front, side, top, detail views) → go to Q5b

#### Q5a: Single-angle isometric renders

The product is shown from one fixed 3D perspective. Renders are pre-generated for each configuration.

*Complexity signal:* This is similar to static images (Q3) in terms of visualization complexity, but with a 3D rendering pipeline for asset production. The configurator technology is **low complexity**; the render pipeline effort depends on the number of combinations.

→ **End of this branch.** Proceed to next factor.

#### Q5b: Multi-angle pre-renders

Multiple viewing angles, each pre-rendered for the available configurations.

*Complexity signal:* This suggests **moderate complexity**. The number of required renders multiplies with each angle added. For products with many options, the render pipeline can become a bottleneck — and maintaining it as the catalog evolves is an ongoing cost. At a certain point, real-time 3D becomes more practical than pre-rendering thousands of image combinations.

*What to watch for:* If you need more than 3-4 angles and have more than ~20 configuration combinations, evaluate whether real-time 3D would be more efficient in the long run.

→ **End of this branch.** Proceed to next factor.

---

### Q6: Real-time 3D approach

The configurator renders the product in an interactive 3D scene — the user can rotate, zoom, and explore the product from any angle. Changes to configuration are reflected immediately in the 3D view.

*Why this matters:* Real-time 3D is the most flexible and immersive visualization approach, but it introduces a significant technology layer: WebGL rendering, 3D model management, performance optimization, and browser compatibility.

- **Products have fixed geometry** — the 3D shape doesn't change, only materials/textures swap → go to Q7
- **Products have variable geometry** — the 3D shape changes based on configuration (different sizes, components, or structural options) → go to Q8

---

### Q7: Fixed-geometry 3D

The 3D model's shape stays the same regardless of configuration. What changes is the surface: materials, colors, textures. Example: a chair that comes in 20 fabric options but always has the same shape.

*Why this matters:* Fixed geometry simplifies the 3D pipeline significantly. You load one model and swap materials — no need to manage multiple geometries or generate shapes dynamically.

- **Few models** (1-5 products, each with one model) → go to Q7a
- **Many models** (10+ products, each needing its own 3D model) → go to Q7b

#### Q7a: Few fixed-geometry models

A small number of products, each represented by one 3D model with material swaps.

*Complexity signal:* This suggests **moderate complexity** for visualization. The core challenge is in model quality, material setup, and web optimization. With few models, the pipeline is manageable and performance is typically straightforward.

*What to watch for:*
- Do 3D models already exist? If so, how much optimization do they need for web delivery?
- How many materials need to be configured? Each material needs proper texture maps (diffuse, normal, roughness at minimum) for convincing results.

→ **End of this branch.** Proceed to next factor.

#### Q7b: Many fixed-geometry models

A larger product catalog, each product needing its own 3D model. All models have material swaps but no geometry changes.

*Complexity signal:* This suggests **moderate-to-significant complexity**. The challenge shifts to the asset pipeline: producing, optimizing, and managing many 3D models. Loading strategies become important — you can't load all models at once. Consistency across models (style, quality, scale) requires art direction.

→ **End of this branch.** Proceed to next factor.

---

### Q8: Variable-geometry 3D

The 3D model's shape changes based on configuration. This could mean selecting from a set of pre-built geometry variants, or generating geometry dynamically.

*Why this matters:* Variable geometry is where 3D configurator complexity takes a significant step up. The system must manage multiple geometries, handle transitions between them, and ensure everything fits together visually.

- **Discrete geometry variants** — the product comes in a set of predefined shapes/sizes, each with its own pre-built 3D model → go to Q8a
- **Parametric geometry** — the 3D shape is generated from code based on user-specified dimensions or parameters → go to Q9

#### Q8a: Discrete geometry variants

The product has predefined size or shape options, each with its own 3D model. Example: a table in S, M, L sizes — three separate models.

*Complexity signal:* This suggests **moderate-to-significant complexity**. It's essentially "many models" (Q7b) combined with the need to transition smoothly between variants. The number of geometry variants multiplied by the number of material options determines the total configuration space.

*What to watch for:* If geometry variants and material options are independent, complexity stays manageable. If certain materials are only available in certain sizes (cross-dependencies), the catalog structure factor compounds the visualization challenge.

→ **End of this branch.** Proceed to next factor.

---

### Q9: Parametric 3D

Geometry is generated dynamically from code, based on user inputs (dimensions, parameters, spatial constraints). Instead of loading a pre-built model, the configurator builds the 3D shape in real time.

*Why this matters:* Parametric rendering handles infinite variation — any dimension, any proportion — but requires writing geometry generation code. The visual development process is fundamentally different from traditional 3D modeling.

- **Simple parametric shapes** — geometry is relatively basic (boxes, cylinders, extruded profiles) with user-specified dimensions → go to Q9a
- **Complex parametric shapes** — geometry involves curves, compound forms, or components that must fit together with precise tolerances → go to Q9b
- **Full parametric system** — multiple parametric components assembled together, each adapting to the others → go to Q9c

#### Q9a: Simple parametric geometry

Basic shapes generated from dimensions. Example: a shelf system where the user specifies width and height, and the configurator generates a rectangular frame with evenly spaced shelves.

*Complexity signal:* This suggests **significant complexity** in visualization. Parametric geometry generation is specialized work, but simple shapes keep the mathematical complexity manageable. The main challenge is ensuring the generated geometry looks good at any dimension — proportions, edge details, and material tiling must all adapt gracefully.

→ go to Q10

#### Q9b: Complex parametric geometry

Shapes involve curves, compound forms, or tight tolerances. Example: a countertop that wraps around a corner with a curved edge profile, adapting to user-specified measurements.

*Complexity signal:* This suggests **high complexity**. Complex parametric shapes require solid geometry knowledge and careful handling of edge cases (what happens at extreme dimensions? At very tight corners?). Testing is inherently harder because the output space is continuous, not discrete.

→ go to Q10

#### Q9c: Full parametric system

Multiple parametric components assembled together, each adapting to the others. Example: a kitchen where parametric cabinets, countertops, and panels all generate their geometry based on the room layout and each other's positions.

*Complexity signal:* This suggests **very high complexity** in visualization. This is essentially a CAD system running in the browser. The geometry generation logic must handle inter-component relationships, gap filling, tolerance management, and edge cases at every connection point. Performance optimization becomes critical as system size grows.

→ go to Q10

---

### Q10: Materials and textures

For any real-time 3D approach, the material and texture pipeline is an important complexity dimension.

*Why this matters:* Materials define how surfaces look — wood grain, fabric weave, metal finish. Where these textures come from and how many there are directly affects both the asset production effort and the runtime performance.

- **Few standard materials** (under ~10 options, common woods/metals/fabrics available from public libraries) → go to Q10a
- **Many standard materials** (10-50 options, mostly sourced from standard libraries) → go to Q10b
- **Custom or proprietary materials** (materials unique to your products — proprietary fabrics, custom finishes, specific stone patterns that need to be scanned or specially produced) → go to Q10c

#### Q10a: Few standard materials

A small set of common materials. Textures are readily available from standard libraries or can be easily produced.

*Complexity signal:* This adds **minimal additional complexity** to the visualization factor. Material setup is straightforward with a small number of standard options.

→ **End of this branch.** Proceed to next factor.

#### Q10b: Many standard materials

A wider range of materials, but still mostly standard types available from existing sources.

*Complexity signal:* This adds **moderate additional complexity**. With many materials, loading strategy matters — you can't preload all textures at once. A material management system (lazy loading, texture atlases, or runtime texture streaming) becomes necessary. Consistency across materials (similar quality level, correct scale) requires attention.

→ **End of this branch.** Proceed to next factor.

#### Q10c: Custom or proprietary materials

Materials unique to your products that don't exist in standard libraries. These may need to be physically scanned, photographed under controlled conditions, or specially created.

*Complexity signal:* This adds **significant additional complexity**. Custom material production is specialized work — scanning physical samples to create convincing digital textures (with correct color, grain direction, scale, and light response) requires equipment and expertise. The results also need careful validation against physical samples.

*What to watch for:* How many custom materials are there? A handful of signature finishes is manageable. Fifty proprietary fabric swatches that must all look accurate is a substantial material production project.

→ **End of this branch.** Proceed to next factor.
