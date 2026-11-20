# Requirements — Life Dashboard (CodingCamp)

> The approved plan. This is the source of truth for what the app must do.

## Overview

A single-page productivity dashboard that runs entirely in the browser.
No backend, no frameworks. All data is saved in the browser's Local Storage.

## Core MVP features

### 1. To-Do List (full)
- [ ] Add a task.
- [ ] Edit an existing task.
- [ ] Mark a task as completed.
- [ ] Delete a task.
- [ ] Save tasks in Local Storage.
- [ ] Load saved tasks when the page opens.
- [ ] Sort tasks (extra usability) — incomplete tasks first.

### 2. Focus Timer
- [ ] Default 25-minute countdown (starts at 25:00).
- [ ] Start button.
- [ ] Stop / Pause button.
- [ ] Reset button.
- [ ] Display time in MM:SS.
- [ ] Show a notification when the timer finishes.

### 3. Quick Links
- [ ] Add favorite website links.
- [ ] Edit links.
- [ ] Delete links.
- [ ] Open links in a new tab.
- [ ] Save links in Local Storage.

### 4. Greeting
- [ ] Display the current time (live clock).
- [ ] Display the current date.
- [ ] Show a greeting based on the time of day (morning / afternoon / evening).
- [ ] Allow a custom user name.

## Three selected challenges

1. **Light / Dark Mode** — a toggle switches themes; the choice is saved.
2. **Custom Name in Greeting** — the user's name is saved and shown in the greeting.
3. **Preventing Duplicate Tasks** — before adding, check the list; warn and block if the task already exists.

## Extra usability

- **Task sorting** — keep incomplete tasks above completed ones.

## Technical constraints

- HTML, CSS, and Vanilla JavaScript only.
- Browser Local Storage only — no backend, no database.
- No frontend frameworks (no React, Vue, etc.).
- Exactly one CSS file: `css/style.css`.
- Exactly one JavaScript file: `js/script.js`.
- Keep the `.kiro` folder for CodingCamp specs.

## Folder rules (CodingCamp)

```
To-Do-List-Life-Dashboard/
├── .kiro/spec/   (requirements.md, design.md, tasks.md)
├── css/style.css
├── js/script.js
├── index.html
└── README.md
```
