# Agent Context — Kanban Board v2

## Project Structure

Single-file SPA: everything in `index.html` (embedded `<style>` and `<script>`).

## Architecture

- **State**: `data` object loaded from/saved to `localStorage` on every mutation. JSON serialization.
- **Rendering**: functional — `renderBoard()` and `renderArchive()` rebuild respective DOM from state on every change.
- **Data helpers**: `loadData()`, `saveData()`, `getCard(columnId, cardId)`, `clone(obj)`, `uid()`.
- **No router** — simple view toggle (`board` vs `archive`) via `display: none/block` on `.view` containers.

## Naming Conventions

- DOM: `kebab-case` for IDs, classes, data attributes
- JS: `camelCase` for variables and functions
- Column IDs: `backlog`, `todo`, `in-progress`, `review`, `done`

## Tokyo Night Palette

Use these exact hex values. Store as CSS variables in `:root`.

### Base
- `--bg`: `#1a1b26`
- `--surface`: `#24283b`
- `--column-bg`: `#1f2335`
- `--card-bg`: `#292e42`
- `--text`: `#c0caf5`
- `--text-muted`: `#565f89`
- `--accent`: `#7aa2f7`
- `--accent-hover`: `#89b4fa`
- `--border`: `#3b4261`

### Per-Column (applied via `--col-color` custom property on `[data-column-id]`)
- Backlog: `#565f89`
- To Do: `#7dcfff`
- In Progress: `#ff9e64`
- Review: `#bb9af7`
- Done: `#9ece6a`

### Secondary action button colors
- Edit hover: `#7dcfff`
- Duplicate hover: `#9ece6a`
- Archive hover: `#ff9e64`
- Delete hover: `#f7768e`

## Key Implementation Patterns

### Drag & Drop (Desktop)
- `dragstart`: set `dragState`, add `.dragging` class
- `dragover`/`dragleave`: toggle `.drag-over` on column
- `drop`: calculate drop index via `getDropIndex()` (Y midpoint), call `moveCardWithinColumn()` or `moveCardToColumn()`
- `dragend`: clean up classes

### Touch (Mobile)
- `touchstart`: clone card element, append to body, set `touchState`
- `touchmove`: position clone at finger, `elementFromPoint` to detect column
- `touchend`: remove clone, resolve drop target, call same move functions

### Card CRUD
- Single shared modal (`#modal-overlay`), populated via `openCreateModal(colId)` or `openEditModal(cardId, colId)`
- `confirmModal()` reads `modalState.mode` to decide create vs update
- Delete uses `confirm()` dialog
- Duplicate deep-clones card, appends at `order + 1`, reorders

### Archive
- `archiveCard()` sets `card.archived = true` and `card.archivedAt = Date.now()`
- `renderBoard()` filters out archived cards
- `renderArchive()` queries all columns for `c.archived`, filters by selected time range

### Archive Navigation
- Three modes: `week` (ISO week, relative offset in 7-day steps), `month` (calendar month), `year` (calendar year)
- `getWeekStart()` returns Monday of the ISO week at given offset
- Prev/next buttons adjust `archiveOffset`

### Settings & Data Management
- **Export**: `JSON.stringify` → `Blob` → `URL.createObjectURL` → anchor click → `revokeObjectURL`
- **Import**: file input → `FileReader` → `JSON.parse` → `validateImportedData()` (strict: valid object, `columns` array of length 5, each column has `id`/`title`/`cards`, each card has `id`/`title`) → inline Replace/Merge/Cancel options
- **Merge**: `clone(impCol.cards).sort((a,b) => a.order - b.order)` to preserve source arrangement; on `id` collision, assign new `uid()`; append all to column end; call `reorderCards()` on each affected column
- **Reset**: modal reused with `modalState.mode === 'reset'`; label/placeholder set to "Type DELETE to confirm"; description field hidden; confirm button becomes `.btn-danger` + disabled until exact match; `confirmModal()` checks `mode === 'reset'` before create/edit; `closeModal()` restores all modal fields to defaults
- **Metadata**: computed in `renderSettings()` on view enter; `Blob` constructor for exact byte size
- **Import state**: `importedData` scoped to settings module; cleared on view re-entry, cancel, or after replace/merge

### View Switching (3 views)
- Board, Archive, Settings; `switchView()` uses generic `view + '-view'` and `'view-' + view + '-btn'` ID pattern; clears `active` from all nav buttons before setting active one
- `renderSettings()` called on settings view entry; existing `render()` updates board + archive only

## When Making Changes

1. Keep everything in a single file unless there's a strong reason to split.
2. Never add build tools or npm dependencies.
3. Follow the Tokyo Night palette for all colors.
4. Ensure both desktop DnD and touch DnD remain functional.
5. Test that localStorage round-trips correctly after any state mutation.
