# Life Dashboard

A browser-based productivity dashboard for the CodingCamp assignment.
Built with plain HTML, CSS, and Vanilla JavaScript. No backend, no frameworks.
All data is saved in the browser's Local Storage.

## Features

- **To-Do List** — add, edit, complete, delete, and sort tasks. Saves to Local Storage.
- **Focus Timer** — 25-minute countdown with start, pause, and reset, plus a finish alert.
- **Quick Links** — save favorite sites, open them in a new tab, edit and delete.
- **Greeting** — live clock, today's date, a time-of-day greeting, and a custom name.

### Three challenges
1. Light / Dark mode (toggle, saved).
2. Custom name in the greeting (saved).
3. Prevent duplicate tasks (warn and block).

### Extra usability
- Task sorting (incomplete tasks first).

## Tech & constraints

- HTML, CSS, Vanilla JavaScript only.
- Browser Local Storage only — no backend, no database.
- No frontend frameworks.
- Exactly one CSS file: `css/style.css`.
- Exactly one JavaScript file: `js/script.js`.
- `.kiro/spec/` holds the CodingCamp specs.

## Folder structure

```
To-Do-List-Life-Dashboard/
├── .kiro/
│   └── spec/
│       ├── requirements.md   # what the app must do
│       ├── design.md         # UI design and layout
│       └── tasks.md          # build stages and checklist
├── css/
│   └── style.css             # the only CSS file
├── js/
│   └── script.js             # the only JavaScript file
├── index.html                # the page
└── README.md                 # this file
```

## How to run

Open `index.html` in any modern browser. No build step, no server needed.

## Build status

- [x] **Phase A** — structure and styling.
- [x] **Phase B** — greeting and clock + custom name.
- [x] **Phase C** — to-do list (add, duplicates, edit, complete, delete, save, sort).
- [x] **Phase D** — focus timer (start, pause, reset, finish alert).
- [x] **Phase E** — quick links (add, edit, delete, open, validation, save).
- [x] **Phase F** — light/dark theme toggle (saved).

All six phases are implemented. See `.kiro/spec/tasks.md` for the full stage checklist.
