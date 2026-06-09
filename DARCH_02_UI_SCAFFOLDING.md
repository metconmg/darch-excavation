# DARCH — UI Scaffolding & Design Philosophy
## For UI Implementation — Feed to Sigma

---

## PART ONE: Interface Inventory

### What DARCH Is

A spatial canvas IDE where the diagram is the program. You design systems by placing nodes, declaring what data enters and exits each one, and wiring them together. The canvas enforces those contracts — it tells you when connections are broken, when logic is stale, and when your architecture is ready to build from.

### Component Glossary

**Canvas** — Infinite grid. Drag to place nodes, draw edges between them, pan and zoom. The working surface.

**Nodes** — Atomic units. Each has a label, a shape, and a type. Double-click to open the interior.

**Workstation** — The node's interior editor. Three panes: Contract In (what arrives), Logic Body (what happens), Contract Out (what leaves).

**Edges** — Connectors between nodes. Carry their own label and schema. Turn red on mismatch, green when committed.

**Contract Library** — Named IO schema registry. Define a shape once, bind it to any edge via dropdown. Validates on intake — nothing malformed enters.

**Integrity Engine** — Audits the graph. Flags orphaned nodes, cycles, half-committed edges, schema mismatches, stale commits, and missing contracts.

**Audit Modes** — Outside→In scans for missing pieces and vague connections. Inside→Out scans for authority leaks and unsafe mutations. Two passes, two different failure classes.

**Draft / Impl Modes** — Draft is for building. Impl activates contract enforcement, schema checking, and the commit gate.

**Commit System** — Nodes and edges go Draft → Live → Stale. Stale means the logic changed after commit. Nothing promotes without passing validation.

**Manifest** — Export surface. Outputs only committed elements as structured JSON or plain text, with the full integrity report attached.

**Navigator** — Left panel. Lists all nodes with live status badges. Click to select, double-click to open workstation.

**SPA Parser** — Upload an HTML file. The canvas reverse-engineers it into nodes and edges, with a preview gate before anything lands.

**Save / Load / Autosave** — Sessions save as `.dag` files. Contract library travels with the session. Autosave runs in the background.

---

## PART TWO: Layout Wireframe

### TOP BAR — full width, fixed

Logo · Draft/Impl mode toggle · Mode pill · Undo/Redo · Audit toggles (Outside→In, Inside→Out) · Integrity · Manifest · Save · Load · Import SPA · Autosave indicator

---

### LEFT PANEL — collapsible, ~220px

Navigator tree — node list with status badges. Slim mode collapses to icon strip with Add Node, Connect, Pan, Integrity shortcut buttons.

---

### CANVAS — center, fills remaining space

Infinite SVG grid. Nodes live here. Edges route between them.

Canvas overlays:
- Audit banner — top center, appears when audit mode is active
- Connect banner — bottom center, appears during edge drawing
- Multi-select bar — bottom right, appears when multiple nodes selected
- Integrity bar — top center, appears after integrity run, auto-dismisses

---

### RIGHT PANEL — collapsible, ~380px, context-sensitive

Opens when a node or edge is selected. Contents shift based on selection and mode.

**Node selected** → Label · Shape picker · Accent color · Logic Env · Status badge · Open Workstation button

**Edge selected** → Label · Bidirectional toggle · Contract Library section (collapsible) · Contract binding dropdowns (Impl mode only) · Schema mismatch indicator · Freehand schema fields

---

### MODALS / OVERLAYS — full screen, stacked by z-index

**Workstation** — Full screen. Three-column: Contract In · Logic Body · Contract Out. Commit button, Save & Close, status badge. Opens on node double-click.

**Manifest** — Centered overlay. Tabbed: Structured JSON · Plain Text. Integrity report at top, committed elements below. Copy and Download.

**Contract Library Modal** — Centered overlay, narrower. Tabbed: Paste/Type · Upload. Required fields with live validation. Save locked until clean.

**SPA Import Preview** — Full screen. Two columns: parsed node cards (left) · parse log (right). Confirmation gate before canvas drop.

**Context Menu** — Small floating menu on right-click. Options: Open Workstation · Commit · Duplicate · Delete.

**Toast** — Bottom center, pill-shaped, auto-dismisses. Status feedback for all actions.

---

### FUTURE UI STATES — speculative, placement defined

**Topbar additions** — Session name field (left of logo area) · Validate All button (alongside Integrity)

**Left panel second tab** — Contract Library browser. Full browsable/searchable schema registry independent of edge selection.

**Left panel third tab** — Layer control. Show/hide node groups by domain or layer.

**Canvas additions** — Minimap (bottom right corner, fixed overlay) · Group/Region annotation boxes (behind nodes, label layer) · Live edge validation dots (midpoint of every edge, always visible in Impl mode)

**Right panel additions** — Node contract status summary (Impl mode, node selected) · Edge history/changelog (collapsible, last commit timestamp + schema at commit)

**New modal: Diff View** — Side-by-side current state vs last committed state. Triggered from Manifest or dedicated topbar button.

**New modal: Build Export Wizard** — Stepped: pick format → select elements → preview → download. The "hand it to Claude cold" surface made explicit.

**New panel: Simulation / Trace** — Bottom drawer. Feed sample input at any node, animate data flow through the graph edge by edge. Contract check per hop. Preflight surface. This is where "does the data transmit flawlessly" lives visually.

**Status bar** — Persistent bottom strip. Node count · Edge count · Committed count · Library schema count · Last saved. Always visible, zero interaction.

---

## PART THREE: Design Philosophy — Accessibility as Design

### Core Principle

This is not an accessibility add-on. It is the design. Every decision — typography, color, motion, spatial rhythm, interaction model — is made once, natively, at the root. There is no standard version and an accessible version. There is one surface built to serve every cognitive profile from the first pixel.

---

### The AI Fatigue Problem

Working in AI-augmented environments introduces a category of cognitive overload that has no precedent in traditional software use. It operates across three dimensions simultaneously.

**Intellectual** — AI produces output at a rate that exceeds human verification speed. The brain is constantly deciding what to trust, what to check, what to absorb. That decision-making is invisible but continuous and compounds over a session.

**Psychological** — The interaction is conversational but not human. The brain partially pattern-matches it to social engagement, which draws on emotional processing resources not recovered the same way task-based focus is. You finish a long AI session feeling something closer to social exhaustion than work exhaustion.

**Emotional** — When AI gets something wrong in a confident voice, or when you cannot tell if what it produced is right, there is a low-grade anxiety that accumulates. Not panic — something quieter and harder to name. It erodes trust in your own judgment over time if the environment does not give you clear ground to stand on.

DARCH's canvas is a direct response to this. The surface holds the truth. The contracts are declared. The status is visible. You are not asking an AI what the state of your system is — you can see it. That is the sovereignty layer made physical.

---

### Design Principles

**Reduce decode load at every surface.**
The user's brain is already working hard in this environment. Every label, every status indicator, every transition should communicate its meaning through the fastest channel available — shape, color, position, motion — before it communicates through text. Text is the slowest channel. Use it last.

**Motion reports. It does not perform.**
No animation exists for delight alone. Every transition carries a semantic meaning. State changes have motion signatures that become readable without conscious attention after repeated exposure. The goal is a canvas you can monitor peripherally while focus is elsewhere. Rejection has a character. Commitment has a character. Staleness has a character. Each is distinct and learnable.

**Color is a vocabulary, not a palette.**
The full semantic color set is locked at the design root and never violated. Green is live. Red is broken. Amber is attention required. Purple is egress boundary. This vocabulary is consistent across every surface, every mode, every modal. After one session the user's brain stops reading labels and starts reading color directly. That is a measurable reduction in cognitive overhead per interaction.

**Typography does cognitive work.**
Font selection prioritizes scanability at small sizes under divided attention — not aesthetics first. The display font for node labels must be instantly parseable at a glance. The monospace font for contracts and logic must be visually distinct from UI chrome so the user always knows what is data and what is interface. Beautiful is a result of this working correctly, not a goal pursued separately.

**Spatial consistency is cognitive rest.**
Grid snap, consistent node sizing, predictable panel behavior, stable interaction patterns — these eliminate micro-decisions. Every time the interface behaves exactly as expected, the brain gets a small rest. Accumulated over a session that rest is meaningful. Unpredictability is tax. Consistency is a refund.

**The keyboard and voice layer is the last mile, not the first.**
Shortcut keys, tap-through navigation, voice actions — these are only effective when the visual logic underneath them is already coherent and spatially consistent. You cannot shortcut your way through an incoherent UI. The polish phase builds the foundation. The interaction layer is built on top of a surface that already makes sense without it.

---

### Application to Specific Cognitive Profiles

**Dyslexia** — The architecture lives in the arrangement of boxes, not in paragraphs. Node labels are short, uppercase, monospaced — high contrast, minimal decoding load. Color and status badges carry meaning without words. You read a DARCH canvas the way you read a map, not the way you read a manual.

**ADHD** — Context is always visible. The canvas holds full state. Every node shows its status. The integrity bar tells you exactly what is unfinished. You never reconstruct context from memory — the surface tells you what is done and what is not. The commit gate creates a natural external definition of done. The contract is either satisfied or it is not. That is not a judgment call — it is a fact on the canvas.

**Accessibility / Motor / Visual** — Keyboard shortcuts cover almost every action. High contrast dark mode with neon accents designed for legibility at a glance. Status system is colorblind-addressable — green committed badge does not rely on green alone, it also carries a checkmark glyph and the word LIVE. Shape and text redundancy alongside color throughout.

---

### The Proof Condition

A user should be able to sit down after a long AI work session — already fatigued, already overloaded — open DARCH, and immediately know the state of their system without reading a single sentence. The canvas tells them. The colors tell them. The motion tells them. The tool does not add to the load. It is the one surface in the environment that gives the load back.

That is the design target. Everything else is in service of that.

---

### For the UI Polish Phase

Start with a typography and color token audit. Lock the full semantic color vocabulary and the type scale before touching a single pixel of layout. Everything else derives from that. The motion language is defined second — each state change gets a motion signature before animation is implemented. The interaction model (keyboard, voice, tap-through) is defined last, on top of a visual surface that already communicates without it.

The north star: music on, head down, the canvas communicating in color and pulse and motion — the user fully in flow, not reading anything, knowing exactly what their system is doing. That is the proof that the design is correct.
