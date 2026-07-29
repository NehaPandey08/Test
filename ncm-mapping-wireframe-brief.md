# Wireframe Brief: NCM/uNCM Mapping Page Redesign (ModelHub)

## Goal

Produce two HTML wireframes for a redesigned NCM/uNCM mapping edit experience in ModelHub. The wireframes are for internal review, not production code. Static HTML/CSS with light JavaScript for interaction states is enough. Keep them clickable so reviewers can feel the flow.

- Wireframe A: select elements and update mappings inline on the same page
- Wireframe B: select elements on page 1, then update all selected mappings together on a next page

## Problem statement

The current mapping edit flow forces too many selections across different hierarchy levels before a single mapping can be updated. Today the user must:

1. Select a data element (for example DE2)
2. Then select a message
3. Then select a path based on that message
4. Only then can they view or edit the mapping

Selecting the next element (DE3) discards the context of the previous one (DE2), so editing multiple mappings in one sitting is impossible. Each edit is a full round trip through the hierarchy.

The team that will use this page currently works in an Excel sheet (the TXA sheet) where everything related to a mapping is visible on one sheet. They scan, click a cell, and type. Selling a tool that replaces one visible sheet with multi-screen, multi-level navigation is very difficult. The redesign must get as close to that spreadsheet mental model as possible: everything visible, edit in place, minimal clicks.

## Context

- Platform: ModelHub, a payment message standards platform. This page covers NCM/uNCM mappings.
- The existing "create message" page in ModelHub is the visual reference for the interaction pattern: rows with checkboxes where selecting a row makes it editable inline. Extend that pattern to mappings.
- The existing read-only mapping view planned three columns per mapping row: source field, inbound path, outbound path. Keep this column model.

## Shared requirements (both wireframes)

**Scope selection (top of page)**
- Select source: message set (for example CIS SMS), version, source message
- Select destination: message set, version, target message
- On load, the grid shows all elements for the selected message pair (for example all data elements for message 1200)

**Grid**
- One row per data element/field
- Columns: checkbox, element identifier and name, inbound (source to target) mapping path, outbound mapping path, notes
- Existing mappings pre-populate their rows; elements with no mapping show empty path cells
- Both inbound and outbound mappings visible together, no toggling between them

**Row selection and editing**
- Checking a row's checkbox makes that row's path and notes cells editable
- Multiple rows can be selected and edited in the same session; selecting a new row never discards edits in another row
- Unchecking a row reverts it to read-only (show a subtle "edited" indicator if it has unsaved changes)

**Path entry with autocomplete**
- Path cells are type-ahead fields
- As the user types (for example "environment" or "cards"), suggest matching paths from the relevant path group for the selected message
- User can pick a suggestion or continue typing; suggestions narrow as they type
- Support add path, update path, and add notes per row

**Save flow**
- Primary actions: Next (to configure/review) and Save
- Show a count of modified rows near the action buttons (for example "4 mappings modified")

**Concurrency**
- Include a "Lock" affordance at the message-type level: when a user starts editing a message pair, that message type is locked for others. Show the locked state in the wireframe (banner or badge is fine). Exact mechanism is out of scope; just represent it visually.

**Design constraints**
- Read and edit concerns should feel clearly separated. Edit mode must be an explicit state (via checkbox selection or an explicit Edit action), never accidental. A reviewer clicking around a read-only view must not be able to modify anything unintentionally.
- Dense, spreadsheet-like layout. Prioritize rows on screen over whitespace.
- Neutral wireframe styling: grayscale, simple borders, system font. No branding.

## Wireframe A: Edit on the same page

- Scope selectors and grid on one page
- Checked rows become editable inline in the grid itself
- Save and Next actions act on all edited rows at once
- Show three states: (1) initial read-only grid, (2) several rows checked and in edit mode with one autocomplete dropdown open, (3) post-save confirmation with modified rows highlighted

## Wireframe B: Select on page 1, edit on the next page

- Page 1: scope selectors plus the read-only grid with checkboxes only; footer button "Edit selected (n)"
- Page 2: an edit screen listing only the selected elements, each with editable inbound path, outbound path, and notes, with the same autocomplete behavior; actions Back, Save, Save and select more
- Show the transition between the two pages and the same autocomplete-open state on page 2

## Out of scope

- Backend/API design, schema changes, actual lock implementation, validation rules
- Create/delete of data elements (covered by existing add/update data element pages)

## Deliverables

- `wireframe-a-same-page.html`
- `wireframe-b-next-page.html`
- A short README noting the interaction assumptions made
