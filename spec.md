# Kanban Board v2

Single-file Kanban board SPA with dark theme, drag-and-drop, and archive time navigation.

## Stack

- **Single HTML file** — all CSS and JS embedded
- **No frameworks or build tools** — vanilla HTML/CSS/JS
- **localStorage** — data persistence (key: `kanban_v2_data`)

## Features

- **5 columns**: Backlog, To Do, In Progress, Review, Done
- **Drag & Drop** — HTML5 DnD API for desktop; touch events for mobile (visual clone + `elementFromPoint`)
- **Card reordering** — drag cards within a column or across columns
- **Card CRUD** — create, edit (modal), delete (confirm dialog), duplicate (with " (copy)" suffix)
- **Archive** — move cards to archive with timestamp; hidden from board
- **Archive page** — toggle via header nav, shows all archived cards with date stamps and source column
- **Archive navigation** — filter by **ISO week** (7-day ranges), **month**, or **year** with prev/next arrows
- **Settings page** — export data as JSON, import with **replace** or **merge** (ID conflict resolves via new `uid()`), reset all data with typed confirmation, metadata display (totals, per-column counts, last modified, data size)
- **Dark theme** — Tokyo Night color palette
- **Per-column accent colors** — each column gets a distinct Tokyo Night color on its top border, header, and title
- **Responsive** — columns stack vertically on screens under 768px

## Color Palette (Tokyo Night)

| Role | Hex |
|------|-----|
| Background | `#1a1b26` |
| Surface | `#24283b` |
| Column bg | `#1f2335` |
| Card bg | `#292e42` |
| Text | `#c0caf5` |
| Muted text | `#565f89` |
| Accent (blue) | `#7aa2f7` |
| Border | `#3b4261` |

### Per-Column Accents

| Column | Color |
|--------|-------|
| Backlog | `#565f89` muted gray |
| To Do | `#7dcfff` cyan |
| In Progress | `#ff9e64` orange |
| Review | `#bb9af7` purple |
| Done | `#9ece6a` green |

## Data Model

```json
{
  "columns": [
    { "id": "backlog", "title": "Backlog", "cards": [...] },
    { "id": "todo", "title": "To Do", "cards": [...] },
    { "id": "in-progress", "title": "In Progress", "cards": [...] },
    { "id": "review", "title": "Review", "cards": [...] },
    { "id": "done", "title": "Done", "cards": [...] }
  ]
}
```

Each card: `id`, `title`, `description`, `order`, `archived`, `archivedAt`, `createdAt`, `updatedAt`.
