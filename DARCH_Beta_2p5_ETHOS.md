# DARCH — Spatial Logic Engine
## Canonical Specification | Beta 2.5 + Ethos
*Last Updated: March 2026 | Status: Living Document*

---

## DESIGN ETHOS — ROOT LAYER
*This section constrains every aesthetic decision in this spec. Build decisions that conflict with this layer are wrong regardless of how reasonable they look in isolation.*

**The canvas is not a sketchpad. It is a build surface and a referee.**
Every visual decision serves one purpose: reduce cognitive load on a brain already operating in an AI-augmented environment. The tool communicates through color, shape, and motion first. Text is the last resort.

**Color is a vocabulary, not a palette.**
Every color token carries a fixed semantic meaning that does not change across surfaces, modes, or contexts. After one session the user stops reading labels and reads color directly.

**Motion reports. It does not perform.**
No animation exists for aesthetic delight alone. Every transition carries a status meaning.

**Text last.**
Labels and tooltips exist to confirm what color and shape already communicated.

**The proof condition:**
A user should open DARCH after a long AI session — fatigued, overloaded — and immediately know the state of their system without reading a single sentence.

---

## ARCHITECTURAL FOUNDATION — LOCKED

### Environment
- **Canvas**: 10,000x10,000 infinite surface — void dark #06060e
- **Grid**: Faint 28px intervals rgba(255,255,255,0.04), snap-to-grid enabled
- **Origin**: Scale and coordinate system centered at 0,0
- **Typography**: IBM Plex Mono for all data, contracts, IDs, status. Space Grotesk for UI chrome and panel labels.

### Semantic Color Vocabulary
*These meanings are absolute. They do not change.*

| Meaning | Color | Hex |
|---------|-------|-----|
| Live / Valid / Pass | Green | #1fd98a |
| Broken / Error / Fail | Red | #f05050 |
| Attention / Advisory | Amber | #f5a623 |
| Active / Selected | Blue | #5b8df8 |
| Stale / Modified after commit | Impl Red | #ff4466 |
| Egress / Boundary | Purple | #a78bfa |

### Design Tokens
| Token | Value | Usage |
|-------|-------|-------|
| --bg-canvas | #06060e | Base surface — void dark |
| --bg-deep | #09091a | Topbar, panels |
| --bg-base | #0e0e1e | Modals, raised surfaces |
| --bg-raised | #161628 | Cards, context menus |
| --grid-line | rgba(255,255,255,0.04) | Grid overlay |
| --text-primary | #f0f0ff | Node labels, primary UI text |
| --text-secondary | #9090c0 | Panel labels, metadata |
| --text-dim | #252548 | Disabled, inactive |
| --node-note | #5b8df8 | Blue: Prompt/Logic strings |
| --node-data | #1fd98a | Green: Spreadsheet/CSV |
| --node-code | #f5a623 | Amber: Scripts/Snippets |
| --node-json | #a78bfa | Purple: Structured objects |
| --node-trigger | #ff4466 | Impl Red: G/A/S triggers |
| --link-schemaless | rgba(255,255,255,0.18) | Dashed 1px: Loose relations |
| --link-schema | #5b8df8 | Solid 2px: Structured flow |
| --link-priority | #ff4466 | Bold 4px + glow: Critical paths |
| --border-subtle | rgba(255,255,255,0.09) | Panel borders, dividers |
| --border-active | rgba(255,255,255,0.18) | Hover states, focus rings |

---

## COMPONENT OUTLINE

### 1. NODE SYSTEM

| Node Type | Color | Hex | JSON Control ID |
|-----------|-------|-----|----------------|
| Note | --node-note | #5b8df8 | 60f76e49... |
| Data | --node-data | #1fd98a | 942740d2... |
| Code | --node-code | #f5a623 | 4ee219f5... |
| JSON | --node-json | #a78bfa | ef822fd3... |
| Generate Trigger | --node-trigger + G badge | #ff4466 | 5656061b... |
| Automate Trigger | --node-trigger + A badge | #ff4466 | 699eedb7... |
| Speculate Trigger | --node-trigger + S badge | #ff4466 | 6dd92393... |

*Triggers share --node-trigger color. The G/A/S badge differentiates which trigger. Color communicates "trigger." Badge communicates "which one."*

#### Node States
| State | Visual Treatment | Trigger Condition |
|-------|-----------------|------------------|
| Empty | 40% opacity, dashed border rgba(255,255,255,0.18) | Newly placed, no content |
| Valid / Committed | 100% opacity, green border #1fd98a, checkmark badge | Content complete, contract satisfied |
| Invalid / Error | Red border #f05050, pulsing red halo 1.2x scale 800ms | Schema failure, broken ref |
| Stale | Impl red border #ff4466, dashed stroke, stale badge | Committed then modified |
| Minimized | Colored circle, 2-3 letter initials on node color background | Node collapsed |
| Hover | Border brightens to rgba(255,255,255,0.30) + tooltip | Cursor over node |
| Selected | Blue glow rgba(91,141,248,0.22), blue border #5b8df8, 2px stroke | Active selection |

#### Node Palette (Persistent Bottom Bar)
- Location: Fixed 60px bar at canvas bottom — background #09091a, top border rgba(255,255,255,0.04)
- Display: Colored circles 20px using node color tokens + 2-3 letter initials in IBM Plex Mono
  - NT (Note/blue), DT (Data/green), CD (Code/amber), JS (JSON/purple), G/A/S (Triggers/impl-red)
- Interaction:
  - Click dot — Modal preview expands upward from bar
  - Drag dot onto canvas — Spawn node at drop location
  - Hover — Tooltip shows full name + function_summary in monospace

---

### 2. NAVIGATION SUITE

| Control | Type | Function | JSON Control ID |
|---------|------|----------|----------------|
| Zoom Slider | Range Input | Adjust canvas scale 0.2x to 2.0x | 07cc6a40... |
| Pan Toggle | Toggle | Enable/disable background drag | 70db6504... |
| Recenter | Button | Reset view to 0,0 | 9844f734... |
| Grid Snap | Canvas Setting | Snap nodes to 28px intervals | 9f95ea9e... |

#### Collapsible Side Panels
| Panel | Default | Width Expanded/Collapsed | Content |
|-------|---------|--------------------------|---------|
| Left Panel | Expanded | 280px / 40px | Flags, Group Summaries, Sequence Order, Validation Output |
| Right Panel | Expanded | 280px / 40px | Node Properties, Connection Metadata, Lane Count Advisory |
| Panel Background | — | #09091a | Border: rgba(255,255,255,0.04) |
| Behavior | Auto-hide optional | Chevron toggle | Click canvas to dismiss (configurable) |

---

### 3. CONNECTION SYSTEM — DIRECTED, LABELED, ID-ONLY

#### Visual Legend
| Category | Verb Examples | Line Style | Color |
|----------|--------------|------------|-------|
| Data Flow | feeds, streams, batches | Solid 2px | Blue #5b8df8 |
| Transformation | parses, maps, filters | Dashed 1px | Green #1fd98a |
| Reference | refers to, informs, depends on | Dotted 1px | Dim white rgba(255,255,255,0.18) |
| Merge/Split | merges into, splits from | Double Solid | Purple #a78bfa |
| Control Flow | triggers, conditions, loops to | Bold 4px + glow | Impl Red #ff4466 |
| Semantic | infers, contextualizes, contains | Wavy 1px | Amber #f5a623 |
| Custom | user-defined | User-defined | User choice |

#### Canvas Visual Layer
What you SEE on a connection:
- The line itself styled per category above
- A short ID label: conn_001 in IBM Plex Mono, color #4848a0, near midpoint

No verb text on canvas. Verb lives in tooltip and right panel only.

#### Connection Data Model
```json
{
  "id": "conn_001",
  "source": "node_A_id",
  "target": "node_B_id",
  "type": "feeds",
  "style": {
    "color": "#5b8df8",
    "line": "solid",
    "width": 2
  },
  "metadata": {
    "schema_hint": "CSV to JSON",
    "validation": "advisory",
    "created_at": "2026-03-26T12:00:00Z",
    "label_position": "source"
  }
}
```

#### Disclosure Pattern
| Interaction | Result |
|-------------|--------|
| Hover line | Tooltip: conn_001 feeds Node B — bg #09091a, IBM Plex Mono |
| Click line | Right Panel opens: full connection metadata |
| Long-press line | Context menu — bg #161628, border rgba(255,255,255,0.09): Edit Verb / Change Style / Delete |
| Search conn_001 | Canvas filters: source + target at full opacity, all others at 20% |

#### Connection Authority Model
```json
{
  "connection_authority": {
    "scope": "tiered",
    "hard_gate": ["SCH-001", "ID-001", "REF-001", "TYP-001", "TRN-001", "CYC-001"],
    "advisory": ["ORP-001", "OUT-001", "VRB-001", "SEQ-001"],
    "node_interaction": {
      "on_connect": "check_compatibility",
      "on_hard_fail": "red_border_block_commit",
      "on_advisory": "pulse_amber_halo"
    }
  }
}
```

#### Connection Dots
- Hidden by default; appear on hover at node cardinal edges N/S/E/W
- 8px circles in matching node color token at 60% opacity
- Non-interactive hint only

#### Lane Limit Advisory
- Per Node: Max 3 inbound + 3 outbound (6 total)
- Exceeding limit: Pulsing amber halo rgba(245,166,35,0.3) — advisory only
- Count displayed in Right Panel on node select

---

### 4. INTERACTION TOOLS

| Tool | Function | JSON Control ID |
|------|----------|----------------|
| Box Selection | Click-drag marquee — blue outline #5b8df8 at 40% opacity | c09897c6... |
| Multi-Drag | Move all selected nodes simultaneously | 2662bb10... |
| Right-Click Lock | Lock positions + group border rgba(255,255,255,0.18) | b89e2308... |

#### Node Long-Press Flow
1. User long-presses any node
2. Floating panel appears:
   - 200px wide, anchored to node
   - Background: #09091a
   - Border: rgba(255,255,255,0.09), border-radius 8px
   - Box-shadow: 0 12px 40px rgba(0,0,0,0.5)
3. Panel Tabs — IBM Plex Mono, 8px, uppercase, letter-spacing .1em:
   - [Node Content]: Edit text/Markdown/script/data
   - [Connections]: List inbound/outbound with ID in matching line color, verb in #9090c0
4. [+ Add Connection] button: Dropdown target node + dropdown verb + confirm

#### Connection Drawing
1. Long-press Source Node, drag toward Target Node
2. Release on Target — line preview appears (dashed, dim white)
3. Radial menu at midpoint — bg #161628, border rgba(255,255,255,0.09)
4. User selects verb — system applies token, generates conn_${timestamp}, stores data
5. ID shown on canvas in IBM Plex Mono dim text. Verb in tooltip only.

#### Flags System
| Flag | Meaning | Color |
|------|---------|-------|
| Green | Module Satisfied | #1fd98a corner badge 12px |
| Yellow | In Progress | #f5a623 corner badge 12px |
| Red | Blocked | #f05050 corner badge 12px |

- Interaction: Click flag to toggle status or add note
- Optional: User places manually

#### Grouping + Summarization
1. Marquee select — blue outline #5b8df8 at 40% opacity
2. Right-click — Lock Group: border 1px solid rgba(255,255,255,0.18), group ID badge top-left in monospace dim text
3. Locked groups move as unit; double-click to edit members
4. Right-click locked group — Summarize: opens summary panel in Left Panel

---

### 5. PIPELINE SEQUENCING — NEW IN 2.5

#### Dependency Highlighting
- Selecting a node auto-highlights upstream/downstream
- Non-related nodes dim to 15% opacity; pipeline nodes hold full opacity + blue rim #5b8df8
- Logic derived from connection_authority.dependency_graph

#### Group Sequencing Tab
1. Highlight pipeline, lock group, open Group Menu, select Sequencing tab
2. Click nodes in desired execution order
3. Dependency Assist auto-includes unresolved dependencies on "feeds/triggers" logic
4. Sequence badge: blue circle #5b8df8, white monospace number, top-right corner of node — Sequencing Mode only
5. SEQ-001 violations: amber pulse advisory, never hard block
6. Sequence stored at Group Level, included in handoff metadata

---

### 6. PIPELINE VALIDATION & ISSUES LIBRARY — NEW IN 2.5

#### Validation Tab
- Green #1fd98a (Valid) / Amber #f5a623 (Advisory) / Red #f05050 (Blocked)
- Results in Left Panel — bg #09091a, IBM Plex Mono entries
- Click any flag: affected nodes/connections highlight, non-affected dim to 20%
- Hard gate on structural failures. Advisory on everything else.

#### DARCH Pipeline Issues Registry
| ID | Severity | Trigger | Message | Anchor | Authority |
|:---|:---------|:--------|:--------|:-------|:----------|
| SCH-001 | Error | Node A output fields != Node B input fields | "Schema mismatch: Node A output does not match Node B input." | conn_001 | Hard gate |
| CYC-001 | Error | Directed path forms closed loop | "Circular dependency detected: nodes form a closed loop." | node_A node_B node_C | Hard gate |
| ORP-001 | Warning | Committed node has no edges | "Node has no connections. It is isolated from the pipeline." | node_D | Advisory |
| SEQ-001 | Error | Node B depends on A but sequenced before A | "Node B depends on Node A but executes before it." | node_A node_B | Advisory + amber pulse |
| TYP-001 | Error | CSV output feeds JSON input without transform | "Type conflict: CSV output cannot satisfy JSON input contract." | conn_002 | Hard gate |
| OUT-001 | Advisory | Node output declared but unused downstream | "Node output is declared but nothing receives it." | node_E | Advisory |
| TRN-001 | Error | Raw data connects to processing node without transform | "Raw data reaches a processing node without a transform step." | conn_003 | Hard gate |
| VRB-001 | Warning | Edge verb mismatches logical relationship | "Connection verb does not match the logical relationship." | conn_004 | Advisory |
| ID-001 | Error | Two or more nodes share the same ID | "Duplicate node ID detected. Canvas integrity is compromised." | node_F | Hard gate |
| REF-001 | Error | Logic body references nonexistent node or field | "Reference to a nonexistent node or field. Pipeline has a broken link." | node_B | Hard gate |

---

## DATA MODELS

### 1. Design Handoff Schema (Figma/JSON)
```json
{
  "artifact_type": "FIGMA_DELIVERY_PACKAGE",
  "design_tokens": {
    "bg_canvas": "#06060e",
    "bg_deep": "#09091a",
    "text_primary": "#f0f0ff",
    "text_secondary": "#9090c0",
    "semantic_green": "#1fd98a",
    "semantic_red": "#f05050",
    "semantic_amber": "#f5a623",
    "semantic_blue": "#5b8df8",
    "semantic_purple": "#a78bfa",
    "semantic_impl": "#ff4466"
  },
  "group_records": [
    {
      "figma_group_label": "Connection Types",
      "source_control_ids": ["link_schemaless", "link_schema", "link_priority"],
      "connection_authority": {
        "scope": "tiered",
        "hard_gate": ["SCH-001", "ID-001", "REF-001", "TYP-001", "TRN-001", "CYC-001"],
        "advisory": ["ORP-001", "OUT-001", "VRB-001", "SEQ-001"]
      }
    }
  ]
}
```

### 2. Runtime State Schema (Application)
```json
{
  "group_id": "group_001",
  "summary_state": {
    "nodes": [
      {"id": "A", "type": "CSV"},
      {"id": "B", "type": "Script"},
      {"id": "C", "type": "JSON"}
    ],
    "connections": [
      {"source": "A", "target": "B", "type": "feeds", "id": "conn_001"}
    ],
    "user_sequence": ["A", "B", "C"],
    "validation": {
      "dependency_conflicts": [],
      "status": "advisory"
    },
    "flags": [
      {
        "type": "advisory",
        "issue_id": "OUT-001",
        "message": "Node output is declared but nothing receives it.",
        "anchor": {"type": "node", "id": "node_C"}
      }
    ]
  }
}
```

---

## BOOTSTRAP INSTRUCTIONS (BETA 2.5)

1. CONSTRUCT THE CANVAS: Rebuild the spatial engine using the feature list above.
2. INITIAL STATE: Canvas launches empty — void dark #06060e, grid visible, no nodes.
3. PROTOCOL: Do NOT place any nodes automatically.
4. USER PROMPT: Once code is generated, stop and ask: "DARCH Beta 2.5 is ready. Where should everything go?"
5. PLACEMENT: Only spawn nodes and draw connections once the user provides coordinates or layout instructions.

---

## REVISION HISTORY
- Beta 2.0: Initial spatial engine, 3 connection types, basic nodes.
- Beta 2.1: UI Refinements — Node Palette, Side Panels, Node States.
- Beta 2.2: Connection System — 7 Visual Categories, ID-only labels.
- Beta 2.3: Interaction Model — Long-press, Radial Menu, Connection Authority.
- Beta 2.5: Pipeline Logic — Sequencing, Validation, Issues Library, Runtime State.
- Beta 2.5 + Ethos: Aesthetic alignment — void dark canvas, semantic color vocabulary, tiered authority model, IBM Plex Mono / Space Grotesk typography, design ethos root layer injected.
