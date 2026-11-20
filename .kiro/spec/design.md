# Design — Life Dashboard

## Direction

A modern, premium productivity dashboard. Calm and focused, with a dark navy
base and a purple accent. Clean typography, generous spacing, and a card layout
that feels like a real dashboard.

## Color system

Colors are stored as CSS variables in `css/style.css`, so the Light/Dark
challenge can switch them with a single `data-theme` attribute.

**Dark theme (default)** — `:root`
- Background: deep navy (`#0b1226`)
- Card surface: navy panel (`#131d3a`)
- Accent: purple (`#a855f7`)

**Light theme** — `[data-theme="light"]`
- Background: soft off-white (`#f3f5fb`)
- Card surface: white (`#ffffff`)
- Accent: purple (`#7c3aed`)

## Typography

- System font stack (Segoe UI / system-ui / Roboto) for body text.
- Monospace stack for the clock and timer (SF Mono / Cascadia Code / Consolas).
- Headings use tighter letter-spacing for a premium feel.

## Layout

- Centered container, max width ~1120px.
- **Header card** (full width): greeting + user name on the left, live clock and
  date on the right, theme toggle button at the far right.
- **Dashboard grid** below the header:
  - Desktop (>= 900px): two columns. To-Do list on the left (tall), Focus Timer
    top-right, Quick Links bottom-right.
  - Mobile (< 900px): single column, stacked in order — To-Do, Timer, Links.

## Cards

Each feature is a `.card` with:
- A header: title (with a small purple accent bar) + optional hint badge.
- A body: the feature's form and list / display.

## Theme toggle

A round button in the header. In Phase F, JavaScript will flip the `data-theme`
attribute on `<html>` between `dark` (default) and `light`, and save the choice
to Local Storage. The CSS variables handle the rest of the look.

## Responsive & accessibility

- Grid collapses to one column on small screens.
- Inputs and buttons have visible focus rings (purple outline).
- Semantic HTML: `header`, `main`, `section`, `form`, `ul`, `time`, `button`.
- The theme toggle and form inputs carry `aria-label`s.

## Phase A scope

Phase A delivers structure and styling only:
- `index.html` with semantic markup and placeholders for greeting, clock, and
  all three cards.
- `css/style.css` with theme variables, layout, cards, inputs, buttons, and
  responsive rules.
- `js/script.js` linked but empty of logic (functionality starts in Phase B).
