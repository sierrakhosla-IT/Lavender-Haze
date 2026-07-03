# Executive Tower — Implementation Guide

**Version 1.0 · Bridges architecture to build.**

This guide is the definitive reference for a developer beginning the build phase of Executive Tower. It is deliberately **implementation-independent** — it describes structure, ownership, data, and behavior, never a framework, a language, or a rendering technique. Its sole purpose is to translate the two immutable sources of truth into a form a builder can map directly to components.

## Sources of truth

Two documents govern everything that follows and outrank this guide wherever they conflict:

- **Product Constitution** — the permanent product specification. Defines the four layers, the attention hierarchy, information flow, and the ten permanent principles.
- **Design System** — the visual constitution. Defines type, color, the three glass tiers, spacing, motion, and interaction states.

The Design System is subordinate to the Constitution; the Constitution is subordinate to nothing. This guide is subordinate to both. Where a decision is not specified here, resolve it by asking a single question the whole product asks: **does it make the room quieter or louder?** Choose quieter.

The mental model is fixed: the workspace is **a room, not a webpage.** The operator enters, orients along the top, and finds their attention drawn to the largest object — the desk. Support sits within arm's reach; storage waits along the wall. Every construction decision defends that experience.

---

## 1. Workspace Composition

The workspace is a single, persistent room. It never becomes a different room — it only grows brighter or quieter. There are no views, tabs, or layouts to switch between. Every permanent region below always exists and always occupies its fixed home, so the operator learns the space once and never relearns it.

The regions read top-to-bottom in strict order of decreasing urgency. This ordering is the load-bearing law of the composition: it may never be reordered, only re-weighted.

### The permanent regions

**A · Status Strip** *(Layer: Orient)*
A single horizontal band across the top. The first thing the eye meets on entry. It answers *"where do I stand?"* in one glance and is **read-only by design** — it reports state, it is never a place to act. Spatial rationale: orientation belongs at the threshold of the room, before any action, exactly as a person reads the top of a page before descending into it.

**B · Mission** *(Layer: Orient)*
A quiet panel beside the Hero, holding long-term context — *why today's work matters*. Spatial rationale: Mission sits adjacent to the Hero so purpose and action are read together. It is the horizon that gives the current commitment its meaning. It changes rarely, so it stays visually still.

**C · Current Work — the Hero** *(Layer: Execute)*
The desk. The single dominant object on screen, occupying the center columns of the grid with the most space, the brightest surface, and the nearest depth. It is the reason the workspace exists. Spatial rationale: the largest object at the visual center is where the eye naturally lands; placing the one active commitment there means the operator's attention is directed, not hunted for. **The Hero is never hidden, collapsed, or displaced.**

**D · Execution Queue** *(Layer: Support)*
Below and quieter than the Hero. Holds what comes next — Now, Next, Later Today, Waiting. Spatial rationale: within arm's reach of the desk but never competing with it. It answers "what happens after this step?" without pulling the eye off the current one.

**E · Context** *(Layer: Support)*
Beside the Hero, at the same quiet weight as the Queue. Holds the reference material the current work draws on. Spatial rationale: a reference shelf beside the desk. Present and legible, pulled toward the operator only when the current work needs it — never a feed pushing to be read.

**F · Drawer** *(Layer: Store)*
Closed by default, along the wall. Holds everything not needed this moment — completed work, tickets, calendar, capture inbox, archive, history. Spatial rationale: storage waits out of sight until summoned. Its existence is what lets the active surface stay ruthlessly minimal — anything can be set down without being lost. It is the pressure-release valve for the entire system.

### How the regions relate

The relationship between regions is one of **deference to the Hero.** Every other region is defined by its relationship to Current Work: Orient frames it, Support feeds it, Store holds what it has finished with. No region is autonomous; none may raise its voice into the room. The composition guarantees that at any moment there is exactly one place the operator's eye is meant to go, and one clear path forward from it.

---

## 2. Component Inventory

Every component the workspace requires is listed below. Each names the layer that owns it, the conceptual data it handles, how it behaves under interaction, and — critically — its **priority under maximum load** (the Stress Test). Ownership is exclusive: a component belongs to exactly one of the four layers. Ambiguous placement is rejected.

### Orient layer

#### Status Strip
- **Purpose** — Instant orientation. A single band that answers "where do I stand?" without requiring a decision.
- **Ownership** — Orient.
- **Inputs / Outputs** — In: mission title, progress state, waiting count, upcoming meetings, current time. Out: **nothing.** It emits no actions; it is read-only.
- **Behavior** — Glanceable and non-interactive. It reports and never becomes a toolbar. An approaching meeting may lift its emphasis slightly, but it never demands action.
- **Priority under load** — Holds its shape. It shows the same fields at any volume; a rising waiting count is a quiet number, never a swelling dashboard. If a metric does not change the next action, it does not appear here.

#### Mission
- **Purpose** — Long-term context; the arc that explains why today matters.
- **Ownership** — Orient.
- **Inputs / Outputs** — In: current mission, journey indicator, long-term objective. Out: nothing actionable in the resting state.
- **Behavior** — Visually quiet and slow-changing. Sits beside the Hero so purpose and action read together. Never competes with the Hero for attention.
- **Priority under load** — Unaffected by volume. Mission is about the horizon, not the day's item count; load never touches it.

### Execute layer

#### Current Work — the Hero
- **Purpose** — The primary focal point of the entire workspace: the single active commitment. *This is the product.*
- **Ownership** — Execute. Load-bearing.
- **Inputs / Outputs** — In: the current work, its current step, why it matters. Out: a **single** Continue action that advances the work.
- **Behavior** — Always the dominant visual object; never hidden, collapsed, or displaced by a modal, notification, or empty state. An interruption *re-points* the Hero at the new commitment and sets the previous one down into the Queue — it never opens a second focal point. Because the Hero states its own current step and why it matters, re-entry after any switch is instant.
- **Priority under load** — **Immutable.** The Hero's size, brightness, and center position are fixed, not proportional to how much else exists. Adding a hundred items to the Drawer changes nothing about it. Under maximum load the workspace still shows exactly one Hero — the busier the day, the more valuable that single focal point becomes.

### Support layer

#### Execution Queue
- **Purpose** — Immediate execution support: what happens after the current step.
- **Ownership** — Support.
- **Inputs / Outputs** — In: sorted items across Now, Next, Later Today, Waiting. Out: a re-pointing of the Hero when an item is promoted to current.
- **Behavior** — Always quieter than the Hero. Its "Now" entry is **the same object as the Hero, not a copy** — a pointer to it, never a duplicate. Waiting items stay quieter than actionable work and surface only when they become actionable again. They pull; they never push.
- **Priority under load** — **Capped.** The visible queue shows a finite Now / Next / Later. Depth beyond that rests in the Drawer. A queue that scrolls forever reintroduces the anxiety the product removes, so overflow moves to storage, not to the surface.

#### Context
- **Purpose** — Reference without distraction; the material the current work draws on.
- **Ownership** — Support.
- **Inputs / Outputs** — In: smart resources, context files, notes, reference material relevant to the Hero. Out: nothing that leads the operator away from the work.
- **Behavior** — Reactive. Follows the Hero automatically, surfacing only what the current step needs and hiding the rest. Recedes when the current work closes. Pulled, never pushed.
- **Priority under load** — Bounded by relevance, not by count. Thirty available resources do not become thirty visible cards; Context shows only what the current Hero requires. The remainder is retrievable, out of the way.

### Store layer

#### Drawer
- **Purpose** — Everything not immediately needed, held safely and retrieved on demand.
- **Ownership** — Store.
- **Inputs / Outputs** — In: completed work, tickets, calendar, the single capture inbox, archive, history. Out: retrieval on explicit request only.
- **Behavior** — Closed by default; always available, never intrusive. Nothing inside it can raise its voice into the workspace.
- **Priority under load** — **The load sink.** This is where volume goes. Its contents may scroll indefinitely because they are out of sight; growth here never touches the active surface. Load is expressed as *depth* inside the Drawer, never as *weight* in the room.

#### Capture
- **Purpose** — A frictionless catch-all: the single point where any thought enters the system.
- **Ownership** — Behaves as an always-available input affordance; its landing place — the review inbox — lives in the Drawer (Store). It is one affordance, not two capture surfaces.
- **Inputs / Outputs** — In: a raw, unsorted thought. Out: one item deposited into the single Drawer inbox for later triage.
- **Behavior** — Always on, always the same. It never asks *where* to file at the moment of capture — that decision is deferred to triage. One capture, one inbox.
- **Priority under load** — Constant. Capture never gets busier or harder; the entire point is that anything can be set down instantly without a decision, regardless of how full the day is.

---

## 3. Information Flow

Work moves in exactly one direction. A thought enters once and travels a single, predictable path. Nothing is entered twice; nothing quietly disappears. The operator can always answer *"where did that go?"* because there is only one place it could be.

### The single forward path

```
CAPTURE  →  QUEUE  →  CURRENT WORK (HERO)  →  COMPLETED  →  ARCHIVE
 enters      sorts          acts                closes       rests
```

Every item travels left to right, never sideways into a parallel system.

### Transition logic

**Capture → Queue.** A thought is caught with zero friction and lands in the single Drawer inbox. At triage — a deliberate, later act — it is sorted into the Queue as Now, Next, Later Today, or Waiting. The capture moment itself carries no filing decision; the transition into the Queue is where sorting happens. *Nothing is ever left stranded in capture: an item is either awaiting triage in the inbox or already placed in the Queue.*

**Queue → Current Work (Hero).** Promotion is a re-pointing, not a copy. When an item becomes the active commitment, the Hero points at that one object; the Queue's "Now" and the Hero are the same object in two roles — the Hero its full expression, the Queue entry a pointer. **If they can ever disagree, the architecture has failed.** Only one item may hold this role at a time.

**Current Work → Waiting (and back).** If the active commitment becomes blocked, it moves into the Queue's Waiting group at a lower volume and the Hero re-points to the next actionable commitment. Waiting items never interrupt; they surface again — pull, not push — only when they become actionable. Nothing is lost in the wait; the item remains in the Queue, merely quieter.

**Current Work → Completed.** When work is finished, it closes into the Drawer's Completed store. It leaves the active surface entirely; it does not linger as a "done" badge competing for attention. The Hero re-points to the next commitment — the desk is never cleared and re-set, only re-pointed.

**Completed → Archive / History.** Completed work rests in History: retrievable, out of the way. This is the terminal state — reachable on demand, never surfaced unbidden.

### Two invariants that keep nothing forgotten

- **Waiting never interrupts.** Blocked work lives in the Queue at a lower volume and pulls forward only when actionable.
- **Resources support, never lead.** Context surfaces alongside the Hero it serves and recedes when that work closes. It is never a destination and never a stage in the flow — it accompanies the flow.

The guarantee: every item has exactly **one home and one forward path.** No item may branch into a parallel system, and no transition may leave an item without a next place to be.

---

## 4. Component Behaviors

Each component must define its behavior for every state it can occupy. The states below are the full vocabulary; a component implements only those that apply to it, but must never invent a state outside this set. All feedback stays inside the periwinkle family — interaction is confirmed by a gentle rise in light or depth, **never by a jump to a saturated alert color.**

| State | Definition | Behavioral rule |
|---|---|---|
| **Idle** | The resting, default appearance. | Calm surface, no emphasis borrowed. The baseline against which every other state is a small, soft departure. |
| **Active** | Being pressed or engaged this instant. | Value darkens slightly, shadow contracts, a 1px press. No scale pop, no ripple. |
| **Focused** | Holds keyboard focus. | A soft-edged lavender halo (≈3px at 60%). Always visible, never a hard high-contrast ring. |
| **Expanded** | Progressive disclosure has revealed depth. | Detail eases open on approach; the component grows to hold it without displacing the Hero. Depth was available, not previously shown. |
| **Collapsed** | Depth is hidden; the calm default. | The quiet surface the operator sees before reaching for detail. This is the normal state for the Drawer and for disclosed detail. |
| **Completed** | Work is finished. | Settles quietly toward Sage (done) and departs the active surface into the Drawer. Sage marks *done*, **never alert**. |
| **Waiting** | Blocked, not actionable now. | Held visually quieter than actionable work. Does not interrupt; surfaces only when it becomes actionable again. |
| **Unavailable** | Present but not currently actionable (disabled). | Recedes — reduced value and contrast, no shadow, no interaction feedback. It is legibly there but clearly out of reach. |
| **Error** | A true system failure. | The *only* place saturated red or amber may appear, and only for genuine failure — never for ordinary validation or status. Everything short of real failure stays within the periwinkle family. |
| **Empty** | No content in this region. | Emptiness is correct, not a failure to fill space. A quiet day shows a single Hero and a short Queue and that is right. No empty-state illustration or prompt may displace or diminish the Hero. |
| **High-density** | The region holds far more than fits. | The surface does **not** grow. Padding never compresses to fit more; overflow scrolls within the section or drops to the Drawer. The visible shape is identical to the low-density state. |

Two behavioral laws apply across all states:

1. **Emphasis is borrowed, never taken from the Hero.** Any state that lifts a component's emphasis borrows it from a quieter layer — no state may ever produce a second object as loud as the Hero.
2. **Motion signals a change of state, never movement for its own sake.** Transitions between these states are slow, soft, and few. One thing moves at a time. Emphasis cross-fades; the Hero settles into place with weight. Nothing springs, flashes, or bounces.

---

## 5. Responsive Philosophy

The architecture adapts across Desktop, Laptop, and Mobile by **preserving the cognitive hierarchy, not the pixel layout.** The ordering — Orient → Execute → Support → Store — is constant on every device. What changes is only how much of the quieter layers is visible at once; the Hero's dominance is invariant.

The governing rule across all sizes: **the Hero is always the single dominant object, and every other region always defers to it.** A smaller screen does not earn the right to shrink the Hero or promote a competing element — it only earns the right to move more of the quiet layers into depth.

**Desktop — the full room.** All permanent regions are visible simultaneously: Status Strip across the top, Mission and Context flanking the Hero, Queue below, Drawer along the wall. The Hero holds the center columns with generous breathing room. This is the canonical arrangement.

**Laptop — the room, tightened by margin.** The same arrangement, with elastic margins drawing in. Margins grow and shrink but the grid does not compress and padding never tightens. Context and Queue may hold slightly less visible depth, with the remainder moving to the Drawer. The Hero's proportion and centrality are untouched.

**Mobile — the room, seen one layer at a time.** The layers stack in their canonical order. The Hero remains the dominant object and the default focus; the operator lands on it. Orient collapses to a glanceable band above it. Support (Queue, Context) becomes reachable just beneath and beside it through progressive disclosure rather than being shown all at once. The Drawer stays closed and summoned. Crucially, the *first thing seen* on mobile is still the Hero — the hierarchy survives the loss of width because it was never dependent on width.

Across every breakpoint, adaptation is a change of **emphasis and disclosure, never of mode.** No device introduces a new tab, view, or navigation model. There is one room; it is merely seen through a narrower or wider frame.

---

## 6. Accessibility Philosophy

Accessibility here is not a compliance layer bolted on — it is the same calm, directed experience delivered through every input method. A workspace that guides attention must guide it for the keyboard operator and the screen-reader operator identically.

**Keyboard navigation.** Every action reachable by pointer is reachable by keyboard. The single Continue action on the Hero is the primary keyboard target and should be immediately actionable on entry. Progressive disclosure is fully keyboard-operable — depth that opens on approach for the pointer opens on focus for the keyboard.

**Focus order.** Focus follows the cognitive hierarchy, not the source order of the markup: **Orient → Hero → Support (Queue, Context) → Store.** The operator tabbing through the room encounters it in the same order the eye is meant to travel. The Hero comes first among actionable elements because it is the one thing that matters now. Focus is always visible as the soft lavender halo defined by the Design System — visible and soft-edged, never a hard high-contrast ring, never invisible.

**Motion reduction.** All motion is decorative-of-state, never load-bearing for meaning — so it can be reduced or removed without losing information. When reduced motion is requested, transitions resolve instantly to their end state; emphasis changes by simple presence rather than cross-fade; the Hero appears in place rather than settling into it. Nothing that was communicated by motion is lost, because motion only ever *signaled* a change the layout already expresses.

**Contrast.** The palette carries meaning through **value, not hue** — things recede by getting lighter and quieter — which means rank stays legible even where color perception varies. Text tiers (Primary, Body, Muted, Quiet) must maintain legible contrast against their surfaces at every glass tier. The one warm accent (terracotta) and the one "done" color (Sage) must never be the *sole* carrier of a distinction; they reinforce a hierarchy that is already expressed by size, position, and value. Saturated alert colors appear only for true system failure, so they never become ambient noise that erodes contrast sensitivity.

The test for every accessibility decision is the product's own: does it keep the room **fully functional and calm** for this operator? A workspace that is operable but agitating has failed the Constitution as surely as one that is calm but unusable.

---

## 7. Stress Test Implementation

An architecture is only as good as its worst day. The workspace must survive its heaviest states — **20+ tasks, 15 tickets, 30 resources** — without abandoning its hierarchy. The governing principle is absolute:

> **Load is expressed as depth, never as weight on the primary surface.**

The visible workspace shows the same shape on its busiest and quietest day. Volume flows into the Drawer and into scrollable depth within each section; it never grows the surface.

### How each region absorbs full load

- **Status Strip (20+ tasks).** Does not swell. A rising count is a quiet number, not a chart. It reports "where you stand" at the same size it always has.
- **Execution Queue (20+ tasks).** The visible queue is **capped** at a finite Now / Next / Later. Task 6 through task 20+ rest in the Drawer, retrievable but not shown. The operator sees a manageable few, not the full count.
- **Context (30 resources).** Reactive filtering means only resources relevant to the current Hero surface. Twenty-nine irrelevant resources are invisible until a Hero needs them. Relevance, not count, bounds what is shown.
- **Drawer (15 tickets + overflow).** The load sink. Tickets, completed work, and queue overflow scroll here indefinitely, out of sight, summoned on demand. Growth here is free — it never touches the room.

### The critical guarantee — the Hero under maximum load

Hierarchy is enforced by **relationship, not by count.** The Hero is dominant because it is the largest, brightest, most central object — and those properties are *fixed*, not proportional to how much else exists. Adding a hundred items to the Drawer changes nothing about the Hero's size or position.

Therefore, under maximum load:

1. The Hero remains exactly one object, unchanged in size, brightness, and center position.
2. No count, alert, or backlog may claim a second focal point. When everything is urgent, the workspace still shows exactly one Hero — making that single choice on the operator's behalf **is** the product's core promise.
3. Padding never compresses to fit more. Overflow scrolls or drops to the Drawer; the grid holds.

**End-of-day planning under load** is a single act of selection, not a construction project: choose tomorrow's Hero, confirm Next, let the rest settle into the Drawer. The architecture must resist turning shutdown into a second work session even when the day was full.

The builder's acceptance test for the stress state: place the busiest-day workspace beside the quietest-day workspace. **If they do not share the same shape and the same single dominant Hero, the implementation has failed.**

---

## 8. Engineering Principles

These are non-negotiable. They are testable, and a "no" on any line is a blocker, not a discussion. They translate the Constitution's laws into standing engineering constraints without prescribing any technique.

1. **State is derived from the Constitution.** The four layers, the single forward path, and the one-Hero rule are the data model — not decoration applied to a generic model. If the code lets two objects claim the "current commitment" role, the model is wrong. The Hero and the Queue's "Now" must be *one object in two roles*, structurally incapable of disagreeing.

2. **One dominant object, always — enforced structurally.** Exactly one Hero exists at any moment. The system must make a second equally-weighted focal point *impossible to construct*, not merely discouraged. Urgency re-points the Hero; it never opens a rival.

3. **Every element resolves to exactly one layer.** Orient, Execute, Support, or Store — no element may be ambiguous or belong to two. Placement is a structural property, decided once, not a runtime guess.

4. **Load increases depth, never surface weight.** Overflow is routed to the Drawer or to scrollable depth. No code path may respond to volume by compressing padding, shrinking the Hero, or adding surface. The visible workspace is invariant to item count.

5. **Nothing is entered twice; nothing is lost.** Every item has one home and one forward path. Capture lands in one inbox. There is no parallel system for any item to slip sideways into.

6. **Transitions prioritize calm over speed.** Motion is slow, soft, and singular — one thing moves at a time, emphasis cross-fades, the Hero settles with weight. Nothing springs, flashes, or bounces. If a motion draws attention to itself rather than to a change of state, it is wrong.

7. **Never block the main thread with heavy work.** Calm is a functional requirement; a stall or jank is a spike in the operator's arousal. Any expensive computation — sorting a full backlog, filtering thirty resources, reconciling the day — must never freeze the room. The surface stays responsive and quiet while depth is computed out of the way.

8. **Adapt emphasis, never introduce modes.** No new tabs, views, or layouts. Responsive and stateful changes re-weight the one room; they never cut to a different one.

9. **Disclose progressively.** The default surface is calm; depth appears only on approach or focus. Complexity is never presented up front.

10. **When in doubt, subtract.** Every option offered is a decision the architecture failed to absorb. The governing test for any code — a feature, a state, a token, a control — is the product's own: *does it make the room quieter or louder?* If louder, it is redesigned or declined. Subtraction is a first-class engineering task.

---

*This guide bridges the Product Constitution and the Design System to the build. It is implementation-independent by intent: every section maps to components without dictating how they are made. Where this guide is silent, the two sources of truth govern — and both answer to a single obligation, to hold the operator's attention on the one thing that matters right now and keep everything else within reach but out of the way.*

**Orient → Execute → Support → Store.**
