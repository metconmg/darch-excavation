# DARCH Beta 2.5 — Canonical Specification
## Spatial Logic Engine | Human-Augmentation Frontier

**Status:** Living Document  
**Last Updated:** April 2026  
**Viewport:** Desktop Web Only (1280px minimum)  
**Philosophy:** LLM as Battery — Constrained Infrastructure

---

## 1. CORE PRINCIPLES (Why This Exists)

### 1.1 The Battery Metaphor
DARCH treats the LLM as **infrastructure** — like electricity, natural gas, or medicine. Powerful, essential, requiring disciplined interfaces. The LLM is a **replaceable generation backend**, not the architecture itself.

### 1.2 Human-Native, AI-Extended
- Human operates at the center
- LLM extends within defined constraints
- All state, logic, and verification reside in the shell
- No dependency on specific LLM provider

### 1.3 Constraint-First Design
Without constraints, AI generation drifts. With constraints:
- Pattern matching becomes reliable
- Components snap together (Tetris/LEGO logic)
- Validation happens at connection time, not runtime
- Backend swapping is seamless

---

## 2. VIEWPORT & ENVIRONMENT (Hard Constraints)

### 2.1 Primary Viewport
- **Target:** Desktop Web
- **Minimum:** 1280×900 pixels
- **Input:** Mouse + Keyboard only
- **Excluded:** Mobile (<768px), Touch-only interfaces

### 2.2 Canvas Specifications
- **Surface:** 10,000×10,000 infinite white (`#ffffff`)
- **Grid:** 20px intervals (`#f0f0f0`)
- **Snap:** Mandatory 20px grid snap for all nodes
- **Origin:** 0,0 coordinate center
- **Zoom:** 0.2x to 2.0x range

### 2.3 Typography
- **Family:** Monospace throughout (SF Mono, Cascadia Code, Roboto Mono)
- **Purpose:** Technical clarity, alignment with logic engine nature

---

## 3. ARCHITECTURAL LAYERS (Checkpoint System)

DARCH is built in **frozen layers**. Each layer exports an API the next layer consumes. No layer regenerates previous layers.

### Layer 1: Data Model (Frozen)
**Exports:**
```
Node {
  id: uuid_v4,
  type: "note" | "data" | "code" | "json" | "generate" | "automate" | "speculate",
  x: integer (grid-snapped),
  y: integer (grid-snapped),
  locked: boolean,
  minimized: boolean,
  content: string
}

Connection {
  id: conn_001 format,
  source: node_id,
  target: node_id,
  verb: one_of_7_verbs,
  metadata: object
}

Group {
  id: uuid_v4,
  nodeIds: array,
  sequence: array,
  summary: string
}
```

### Layer 2: Canvas System
- Coordinate space management
- Zoom/pan mechanics
- Grid rendering
- **Does not include:** nodes, connections, UI panels

### Layer 3: Render Primitives
- Node visual rendering (colors, shapes, states)
- Connection line rendering (SVG)
- Panel rendering (sidebars, palette)
- **Does not include:** interaction logic, validation

### Layer 4: Interaction Model
- Drag/drop mechanics
- Selection (shift+click multi-select)
- Connection drawing (drag + radial menu)
- Long-press floating panel
- **Does not include:** business logic, validation rules

### Layer 5: Logic Engine
- Sequencing validation
- Schema validation (10-issue registry)
- Flag system
- Grouping logic
- **Does not include:** rendering, canvas mechanics

### Layer 6: LLM Integration (LLMC)
- G/A/S trigger handlers
- Library pattern matching
- Generation constraints
- **Does not touch:** core physics, validation rules, locked nodes

---

## 4. NODE SYSTEM (7 Types)

### 4.1 Type Definitions
| Type | Color | Function | Shape |
|------|-------|----------|-------|
| **Note** | `#3B82F6` (Blue) | Prompt/logic strings | Rounded rect |
| **Data** | `#10B981` (Emerald) | Spreadsheet/CSV | Rounded rect |
| **Code** | `#F59E0B` (Amber) | Script execution | Rounded rect |
| **JSON** | `#EAB308` (Yellow) | Structured objects | Rounded rect |
| **Generate** | `#000000` (Black) | Initiates generation | Circle (G) |
| **Automate** | `#000000` (Black) | Triggers workflow | Circle (A) |
| **Speculate** | `#000000` (Black) | Speculative branches | Circle (S) |

### 4.2 Node States
| State | Visual | Meaning |
|-------|--------|---------|
| **Empty** | 40% opacity, dashed border | New, no content |
| **Valid** | 100% opacity, solid | Content complete, type-correct |
| **Invalid** | Pulsing red halo | Type error, schema violation |
| **Minimized** | 2-3 initials only | Collapsed for clarity |
| **Hover** | Elevation + tooltip | Focus indicator |
| **Locked** | Black border | Position frozen, content editable |

### 4.3 Node Palette (Bottom Bar)
- **Height:** 60px fixed
- **Position:** Bottom of viewport
- **Items:** 20px colored dots with 2-3 letter labels (NT, DT, CD, JS, G, A, S)
- **Interaction:** Click spawns at center, drag-and-drop to canvas

---

## 5. CONNECTION SYSTEM (The Keystone)

**Without valid connections, the canvas collapses into a static diagram.**

### 5.1 The 7 Verbs (Fixed Vocabulary)
Connections are **action words**, not labels. They define what happens when data flows.

| Category | Verbs | Line Style | Color |
|----------|-------|------------|-------|
| **Data Flow** | feeds, streams, batches | Solid | Blue `#3B82F6` |
| **Transformation** | parses, maps, filters | Dashed | Green `#10B981` |
| **Reference** | refers to, informs, depends on | Dotted | Gray `#94A3B8` |
| **Merge/Split** | merges into, splits from, joins with | Double solid | Purple `#8B5CF6` |
| **Control Flow** | triggers, conditions, loops to | Bold solid | Red `#EF4444` |
| **Semantic** | infers, contextualizes, contains | Wavy | Orange `#F59E0B` |
| **Custom** | [user-defined] | User-defined | User choice |

### 5.2 Connection Syntax
```
[Source Node] [Verb] [Target Node]
```
- Direction = flow direction
- Label near source (verb visible)
- ID label on canvas (conn_001)

### 5.3 Visual Layer (Canvas)
- **See:** Styled line, short ID (conn_001)
- **Don't see:** Full verb text (clutters)
- **Hover:** Tooltip shows `conn_001 • feeds → Node B`
- **Click:** Right panel opens full metadata

### 5.4 Lane Limits (Advisory)
- **Max:** 3 inbound + 3 outbound per node (6 total)
- **Exceeding:** Amber pulse warning (does not block)
- **Count:** Displayed in right panel

### 5.5 Connection Authority (Advisory-Only)
```json
{
  "scope": "advisory",
  "capabilities": ["validate_schema", "suggest_transform", "log_flow"],
  "on_mismatch": "pulse_amber_halo"
}
```
- **Never blocks** user action
- **Always informs** via visual feedback
- User can force "invalid" connection (overrides with warning)

---

## 6. VALIDATION & ISSUES REGISTRY

### 6.1 Philosophy
Validation checks **physics**, not **intent**. Green means "system allows," not "this is correct." Human judgment (flags) bridges the gap.

### 6.2 Severity Levels
| Flag | Meaning | System Role |
|------|---------|-------------|
| 🟢 Green | Satisfied / Ready | Physics valid + verb implemented |
| 🟡 Yellow | In Progress / Pending | Connection exists, action undefined |
| 🔴 Red | Blocked / Error | Physical violation (cycle, crash) |

**Note:** Yellow is the working state. Green is completion. Red is impossibility.

### 6.3 DARCH Issues Registry (Canonical)
| ID | Severity | Trigger | Message |
|----|----------|---------|---------|
| SCH-001 | Error | Schema mismatch | "Node A output does not match Node B input" |
| CYC-001 | Error | Circular dependency | "Nodes form closed loop" |
| ORP-001 | Warning | Isolated node | "Node has no connections" |
| SEQ-001 | Error | Sequence violation | "Node B depends on A but executes before" |
| TYP-001 | Error | Type conflict | "CSV cannot satisfy JSON contract" |
| OUT-001 | Advisory | Unused output | "Node output declared but unused" |
| TRN-001 | Error | Missing transform | "Raw data reaches processor without transform" |
| VRB-001 | Warning | Verb mismatch | "Connection verb doesn't match logic" |
| ID-001 | Error | Duplicate ID | "Duplicate node ID detected" |
| REF-001 | Error | Broken reference | "Reference to nonexistent node/field" |

---

## 7. SEQUENCING & GROUPING

### 7.1 Group Sequencing
1. Highlight pipeline (including dependencies)
2. Lock group → Open Group Menu → Select "Sequencing" tab
3. Click nodes in execution order
4. **Dependency Assist:** Auto-includes unresolved dependencies
5. **Visual:** Sequence badges (1, 2, 3...) on nodes

### 7.2 Execution Rules
- Sequence stored at **Group level**, not individual nodes
- Respects connection direction (cannot sequence B before A if A "feeds" B)
- **Override:** Manual reorder allowed with advisory warning

### 7.3 Grouping Mechanics
- **Select:** Marquee or shift+click
- **Lock:** Right-click → "Lock Group" (black border, group ID)
- **Move:** Locked groups move as unit
- **Edit:** Double-click to edit members

---

## 8. SIDE PANELS (Dismissible, Not Collapsible)

### 8.1 The Fix (Avoiding the Trap)
**Wrong:** Collapse to 40px stub (wastes space, dead pixels)  
**Right:** Fully dismissible to 0px, or auto-hide with edge-hover

### 8.2 Left Panel
- **Expanded:** 280px
- **Collapsed:** 0px (completely gone, canvas extends)
- **Content:** Flags, Group Summaries, Sequence Order, Validation Output
- **Reopen:** Hotkey `[` or edge-hover (8px trigger zone)

### 8.3 Right Panel
- **Expanded:** 280px
- **Collapsed:** 0px
- **Content:** Node Properties, Connection Metadata, Lane Count Advisory
- **Reopen:** Hotkey `]` or edge-hover

### 8.4 Behavior
- No persistent stub
- No dead pixels
- Canvas reclaims all space when dismissed

---

## 9. LLM CONTAINMENT (LLMC) & G/A/S TRIGGERS

### 9.1 The Cage
The LLM is **boxed in**. It can only touch what the system allows.

### 9.2 G/A/S — The Three Gates
| Trigger | Function | LLM Permissions |
|---------|----------|-----------------|
| **G** — Generate | Creates nodes from prompts | WRITE: empty nodes, READ: library |
| **A** — Automate | Executes validated pipelines | READ: green pipelines, WRITE: execution output |
| **S** — Speculate | Explores branches/what-ifs | WRITE: branch nodes (yellow), READ: current state |

### 9.3 LLM Cannot Touch
- Locked nodes (human intent)
- Validation rules (core physics)
- The 7 verbs (fixed vocabulary)
- Flag placement (human judgment only)
- Checkpoint exports (state ownership)
- Connection schema (system enforcement)

### 9.4 Pattern Matching (Library Integration)
- LLM suggests library components based on slot shapes
- "This gap needs CSV Parser → JSON Map → API Runner"
- Human verifies fit (Tetris logic)
- Snap validated components together

---

## 10. COMPONENT LIBRARY

### 10.1 Philosophy
**Bespoke → Pattern → Library → Scale**

Pre-built node groups (2-3 nodes) that snap together:
- CSV Parser → JSON Transformer → API Runner
- Data Validator → Filter → Branch
- Trigger → Condition → Action

### 10.2 Library Characteristics
- **Curated:** Human-verified, not LLM-generated
- **Schema-defined:** Inputs/outputs explicitly typed
- **Verb-matched:** Fits the 7-verb system
- **Visual:** Drag from library, drop on canvas, auto-connect if slots align

### 10.3 Tetris Logic
- Slot defines what's possible
- Piece either fits or doesn't
- Wrong shape = visual rejection (no force possible)
- Right shape = snap, validation green

---

## 11. DATA MODELS

### 11.1 Design Handoff Schema (Figma/JSON)
```json
{
  "artifact_type": "FIGMA_DELIVERY_PACKAGE",
  "group_records": [{
    "figma_group_label": "Connection Types",
    "source_control_ids": ["link_schemaless", "link_schema", "link_priority"],
    "connection_authority": {
      "scope": "advisory",
      "capabilities": ["validate_schema", "suggest_transform"]
    }
  }]
}
```

### 11.2 Runtime State Schema (Application)
```json
{
  "group_id": "group_001",
  "summary_state": {
    "nodes": [{"id": "A", "type": "CSV"}],
    "connections": [{"source": "A", "target": "B", "type": "feeds"}],
    "user_sequence": ["B", "A", "C"],
    "validation": {
      "dependency_conflicts": [],
      "status": "blocked"
    },
    "flags": [{
      "type": "error",
      "anchor": {"type": "connection", "id": "conn_001"}
    }]
  }
}
```

---

## 12. BUILD METHODOLOGY

### 12.1 Checkpoint Pipeline
1. **Build Layer N** → Export → Verify → **Checkpoint N**
2. **Import Checkpoint N** → Build Layer N+1 → Export → **Checkpoint N+1**
3. **Never regenerate** previous layers
4. **If blocked:** State blocker, stop, don't fix previous layer

### 12.2 Constraint Contract (For LLM Generation)
```
ROLE: Code extension engine. Extend only, do not rewrite.
INPUT: checkpoint_vN.html (FROZEN)
PERMISSIONS: READ all, WRITE new functions only
FORBIDDEN: Modifying existing node system, drag logic, grid behavior
OUTPUT: Complete HTML with all previous code UNCHANGED
```

### 12.3 Verification Checklist
- [ ] All Layer 1 functions present?
- [ ] No "improved" or "cleaner" rewrites?
- [ ] New code clearly demarcated?
- [ ] File runs without errors?
- [ ] No prohibited phrases ("refactored", "optimized", "better way")?

---

## 13. TEAM SCALING

### 13.1 Collaborative Canvas
- **Shared memory space:** Canvas state
- **Individual cognition:** Each person's nodes
- **Coordination:** Connection contracts
- **Integration:** Real-time validation

### 13.2 Domain Separation
- Person A: Data ingestion layer (nodes 1-10)
- Person B: Transformation logic (nodes 11-20)
- Person C: Output/Action layer (nodes 21-30)
- **Validation ensures:** Their connections fit (slot shapes match)

### 13.3 Debug-While-Building
- Validation runs continuously
- Errors surface spatially (red halos on canvas)
- "Why is my node red?" → "Doesn't match Sarah's schema upstream"
- Conversations happen on canvas, not in Slack

---

## 14. WHY THIS ARCHITECTURE

### 14.1 The Problems It Solves
| Problem | DARCH Solution |
|---------|----------------|
| LLM drift | Frozen layers, contractual constraints |
| Vendor lock-in | Swappable backend (battery metaphor) |
| Team coordination | Visual consensus, slot-based fit |
| Validation delay | Real-time, spatial, advisory |
| Intent vs. system | Flags for human judgment |
| Ambiguous viewport | Desktop-only, hard constraints |
| Collapsible panel trap | Fully dismissible (0px), no stubs |
| Code regeneration | Checkpoint system, import/export |

### 14.2 The Result
**Build twice at twice the speed** — because:
- Components snap together (Tetris/LEGO)
- Validation happens visually, immediately
- LLM suggests, human verifies
- Backend is replaceable
- Team coordinates via canvas, not documents

---

## 15. BOOTSTRAP PROTOCOL

1. **Construct Canvas:** Rebuild spatial engine using Layer 1-3 specs
2. **Initial State:** Empty canvas (no auto-placed nodes)
3. **Protocol:** Do NOT place nodes automatically
4. **User Prompt:** "DARCH Beta 2.5 Framework is ready. Where should everything go?"
5. **Placement:** Only spawn nodes once user provides coordinates or layout instructions

---

## APPENDIX: Design Tokens Reference

| Token | Value | Usage |
|-------|-------|-------|
| `--bg-canvas` | `#ffffff` | Base surface |
| `--grid-line` | `#f0f0f0` | Grid overlay |
| `--text-primary` | `#1e293b` | Node labels, UI text |
| `--node-note` | `#3B82F6` | Blue: Prompt/Logic |
| `--node-data` | `#10B981` | Emerald: Spreadsheet/CSV |
| `--node-code` | `#F59E0B` | Amber: Scripts |
| `--node-json` | `#EAB308` | Yellow: Structured objects |
| `--node-trigger` | `#000000` | Black: G/A/S triggers |
| `--link-schemaless` | `#94a3b8` | Dashed, 1px: Loose relations |
| `--link-schema` | `#475569` | Solid, 2px: Structured flow |
| `--link-priority` | `#4f46e5` | Bold, 4px + glow: Critical paths |

---

**Document Control:** This specification is the canonical reference for DARCH Beta 2.5. All implementations must conform to the checkpoint system, viewport constraints, and validation registry defined herein.

**End of Specification**
