# k2 — Kanban Board v2

Single-file Kanban board SPA with a dark Tokyo Night theme, drag-and-drop, card archiving, and full data management.

## Features

- **5 columns**: Backlog, To Do, In Progress, Review, Done
- **Drag & drop** — desktop (HTML5 DnD) and mobile (touch events with visual clone)
- **Card CRUD** — create, edit, duplicate, delete
- **Archive** — move cards to archive with timestamp; browse by week/month/year
- **Settings** — export/import JSON, merge imports, reset all data, database metadata
- **Tokyo Night palette** — per-column accent colors
- **Responsive** — stacks vertically on mobile
- **No dependencies** — vanilla HTML/CSS/JS in a single file

## Usage

Open `index.html` in any modern browser. Data persists in `localStorage`.

## Stack

- **Language**: Vanilla HTML/CSS/JS
- **Frameworks**: None
- **Storage**: `localStorage` (key: `kanban_v2_data`)
