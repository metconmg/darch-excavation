# DARCH v7 — Contract Library
## Component Reference

---

### What It Is

The Contract Library is a named IO schema registry built into DARCH v7. It solves the problem of freehand contract entry — where a missing bracket or a mistyped field creates silent corruption in the build surface. Every schema enters through a validated gate. Nothing malformed lands in the library. Nothing in the library is trusted blindly — it was checked on the way in.

The library is not a documentation tool. It is a vocabulary. You define your IO contract shapes once, give them names, and the entire canvas speaks that vocabulary consistently. Dropdowns replace freehand text. The connector becomes the referee.

---

### Where It Lives

Right panel — appears as a collapsible section when an edge is selected. Collapsed by default. Expands via chevron. Sits above the edge schema fields.

Only surfaces on edge selection. Not visible when a node is selected or when nothing is selected. This is intentional — the library is an edge-level tool. Contracts live on connectors, not on nodes in isolation.

---

### Data Structure

Each library entry contains:

```json
{
  "id": "user_input_envelope",
  "name": "User Input Envelope",
  "version": "1.0",
  "desc": "Wraps raw user text into a structured envelope for routing",
  "shape": {
    "value": "string",
    "source": "string",
    "confirmed": "boolean"
  }
}
```

**id** — snake_case, lowercase, no spaces, must start with a letter. Locked after save. Cannot be changed on edit — existing edge bindings depend on it.

**name** — display name shown in dropdowns and the library list.

**version** — semver format (1, 1.0, 1.2.3). Required. Allows multiple versions of the same contract to coexist.

**desc** — optional. One sentence describing what this contract represents.

**shape** — the actual IO contract. A flat JSON object where every key is a field name and every value is a type string. Valid types: `string`, `number`, `boolean`, `object`, `array`, `any`, `null`, `integer`, `float`.

---

### Intake Routes

**Paste / Type tab** — User types or pastes JSON directly into the shape textarea. The validator runs on every keystroke. The shape field shows a green border and a field-count confirmation when valid. Red border and specific error message when invalid. The Save button stays disabled until all required fields pass and the shape is clean.

**Upload tab** — User uploads a `.json` file. Two formats accepted:

*Bare shape* — a plain JSON object of field/type pairs. Parser validates the shape and populates the shape field. User still fills in ID, name, version manually.

*Full template* — a JSON file containing `id`, `name`, `version`, `desc`, and `shape` keys. Parser detects the full template format, validates the shape field specifically, and auto-populates all form fields. User reviews and confirms.

Both routes run identical validation before anything is written to the library.

---

### Validation Rules

The validator enforces in sequence:

1. Input must be valid JSON — no parse errors, no missing brackets, no trailing commas
2. Top-level value must be a JSON object — not an array, not a string, not null
3. Object must contain at least one key
4. Every value must be a recognized type string from the approved set
5. ID must match `^[a-z][a-z0-9_]*$` — no uppercase, no spaces, no special characters
6. Version must match semver pattern
7. Name must be non-empty

Any failure surfaces a specific error message. The save button does not enable until all seven rules pass. There is no override.

---

### Edge Binding

When an edge is selected in Impl mode, two dropdowns appear in the right panel under Contract Bindings:

**Output Contract** — what leaves the source node
**Input Contract** — what the target node expects to receive

Both dropdowns populate from the library. Selecting a schema on both sides triggers an immediate match check using `schemaMatch`. Results:

- Both sides empty — no indicator shown
- One side selected — no indicator shown
- Both sides selected, shapes compatible — green border on both dropdowns, confirmation message with field count
- Both sides selected, shapes incompatible — red border on both dropdowns, field-level diff showing exactly which keys don't match

The edge on the canvas updates its color in real time to reflect the match state. No integrity run required. The canvas is the live referee.

---

### Persistence

The library serializes into the `.dag` session file alongside nodes and edges. Load a session — library loads with it, all entries intact, all edge bindings restored. The library does not save separately. It travels with the diagram.

If a library entry is deleted, all edge bindings pointing to that entry ID are cleared automatically. Schema fields on those edges are reset to empty. The edges are not deleted — they remain on the canvas but their contract binding is unset.

---

### Keyboard / Interaction

- Library section header is clickable — toggles expand/collapse
- Each entry is clickable — opens edit modal for that entry
- Delete button (✕) on each entry — removes with immediate effect, no confirmation modal
- New Schema button — opens intake modal in new mode
- Upload button — opens intake modal defaulted to upload tab
- Cancel in modal — closes without saving, no state change
- Escape key — closes modal if open

---

### Integration Points

**serState / deserState** — library array is included in session serialization. Version key is `7`. Sessions from v6.5 load cleanly (library will be empty on first load from a v6.5 file).

**Integrity Engine** — the existing `schemaMatch` function is used for contract comparison. No new validation logic. The library provides the shapes; the existing engine does the comparison.

**Manifest** — contract library entries are not currently exported in the manifest. Edge bindings (which library entry is bound to which side) are stored on the edge object as `contractOutId` and `contractInId`. These are included in the serialized edge data.

---

### What It Does Not Do

- Does not validate that a contract schema is semantically correct for its use case — only that it is structurally valid JSON with recognized types
- Does not enforce that every edge has a contract bound — unbound edges are valid in Draft mode
- Does not auto-bind contracts based on node type or label — all binding is explicit and manual
- Does not version-diff two entries with the same ID — adding a schema with an existing ID in edit mode replaces it in place
