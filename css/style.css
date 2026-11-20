/* ==========================================================================
   Life Dashboard — script.js
   --------------------------------------------------------------------------
   The single JavaScript file for the whole project (CodingCamp rule).

   Implemented so far:
     Phase B  -> greeting, live clock, current date, custom name
     Phase C  -> to-do list: add, edit, complete, delete, duplicates, save, sort, stats
     Phase D  -> focus timer: start, pause, resume, reset, timestamp accuracy, completion
     Phase E  -> quick links: add, edit, delete, open, URL validation, duplicates, save

   Still to come (not yet implemented):
     Phase F  -> light/dark theme toggle

   Everything is stored in the browser's Local Storage. No backend.
   ========================================================================== */

(function () {
  'use strict';

  /* ---------- Local Storage keys ---------- */
  const NAME_KEY = 'lifeDashboard.name';
  const TASKS_KEY = 'lifeDashboard.tasks';
  const LINKS_KEY = 'lifeDashboard.links';
  const THEME_KEY = 'lifeDashboard.theme';

  // 25 minutes in milliseconds. Lower this number temporarily for quick testing.
  const TIMER_DURATION = 25 * 60 * 1000;

  /* ---------- Element references (IDs match index.html) ---------- */
  const greetingEl = document.getElementById('greeting');
  const nameEl = document.getElementById('user-name');
  const clockEl = document.getElementById('clock');
  const dateEl = document.getElementById('date');

  const todoForm = document.getElementById('todo-form');
  const todoInput = document.getElementById('todo-input');
  const todoList = document.getElementById('todo-list');
  const todoWarning = document.getElementById('todo-warning');
  const todoCount = document.getElementById('todo-count');
  const todoStats = document.getElementById('todo-stats');
  const statTotal = document.getElementById('stat-total');
  const statDone = document.getElementById('stat-done');
  const statPending = document.getElementById('stat-pending');

  const timerTime = document.getElementById('timer-time');
  const timerStart = document.getElementById('timer-start');
  const timerPause = document.getElementById('timer-pause');
  const timerReset = document.getElementById('timer-reset');
  const timerMessage = document.getElementById('timer-message');

  const linksForm = document.getElementById('links-form');
  const linkNameInput = document.getElementById('link-name');
  const linkUrlInput = document.getElementById('link-url');
  const linksList = document.getElementById('links-list');
  const linksWarning = document.getElementById('links-warning');

  const themeToggle = document.getElementById('theme-toggle');
  const themeIcon = themeToggle ? themeToggle.querySelector('.theme-toggle__icon') : null;

  /* ---------- Icon SVGs (Feather icons, MIT license) ---------- */
  const ICON_EDIT =
    '<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true"><path d="M11 4H4a2 2 0 0 0-2 2v14a2 2 0 0 0 2 2h14a2 2 0 0 0 2-2v-7"/><path d="M18.5 2.5a2.121 2.121 0 0 1 3 3L12 15l-4 1 1-4 9.5-9.5z"/></svg>';
  const ICON_DELETE =
    '<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true"><polyline points="3 6 5 6 21 6"/><path d="M19 6v14a2 2 0 0 1-2 2H7a2 2 0 0 1-2-2V6m3 0V4a2 2 0 0 1 2-2h4a2 2 0 0 1 2 2v2"/><line x1="10" y1="11" x2="10" y2="17"/><line x1="14" y1="11" x2="14" y2="17"/></svg>';

  /* ---------- State ---------- */
  let currentName = loadName();
  let lastGreeting = '';
  let lastDate = '';
  let tasks = loadTasks();
  let warningTimer = null;

  let timerRemaining = TIMER_DURATION;
  let timerEndTime = null;
  let timerInterval = null;
  let timerDone = false;

  let links = loadLinks();
  let linksWarningTimer = null;

  /* ======================================================================
     Phase B — Name
     ====================================================================== */

  function loadName() {
    const stored = localStorage.getItem(NAME_KEY);
    const value = stored ? stored.trim() : '';
    return value || 'friend';
  }

  function saveName(name) {
    localStorage.setItem(NAME_KEY, name);
  }

  function renderName() {
    nameEl.textContent = currentName;
  }

  // Click the name -> show an input in its place -> Enter/blur saves, Esc cancels.
  function startNameEdit() {
    if (nameEl.querySelector('input')) return; // already editing

    const input = document.createElement('input');
    input.type = 'text';
    input.value = currentName;
    input.maxLength = 30;
    input.setAttribute('aria-label', 'Your name');

    Object.assign(input.style, {
      fontFamily: 'inherit',
      fontSize: '0.95rem',
      fontWeight: '600',
      color: 'var(--accent)',
      background: 'var(--bg-elevated)',
      border: '1px solid var(--border)',
      borderRadius: '10px',
      padding: '0.3rem 0.55rem',
      width: '14rem',
      outline: 'none',
      boxShadow: '0 0 0 3px var(--accent-subtle)'
    });

    nameEl.textContent = '';
    nameEl.appendChild(input);
    input.focus();
    input.select();

    let finished = false;

    function finish(commit) {
      if (finished) return;
      finished = true;
      const value = input.value.trim();
      if (commit && value) {
        currentName = value;
        saveName(currentName);
      }
      renderName();
    }

    input.addEventListener('keydown', (e) => {
      if (e.key === 'Enter') { e.preventDefault(); finish(true); }
      else if (e.key === 'Escape') { e.preventDefault(); finish(false); }
    });
    input.addEventListener('blur', () => finish(true));
  }

  /* ======================================================================
     Phase B — Greeting + clock
     ====================================================================== */

  // Before 12:00 -> morning, 12:00-17:59 -> afternoon, 18:00-23:59 -> evening.
  function getGreeting(hour) {
    if (hour < 12) return 'Good morning';
    if (hour < 18) return 'Good afternoon';
    return 'Good evening';
  }

  function pad(n) {
    return String(n).padStart(2, '0');
  }

  function updateClockAndGreeting() {
    const now = new Date();

    clockEl.textContent =
      pad(now.getHours()) + ':' + pad(now.getMinutes()) + ':' + pad(now.getSeconds());

    const dateStr = now.toLocaleDateString(undefined, {
      weekday: 'long',
      year: 'numeric',
      month: 'long',
      day: 'numeric'
    });
    if (dateStr !== lastDate) {
      lastDate = dateStr;
      dateEl.textContent = dateStr;
    }

    const g = getGreeting(now.getHours());
    if (g !== lastGreeting) {
      lastGreeting = g;
      greetingEl.textContent = g;
    }
  }

  /* ======================================================================
     Phase C — To-Do list
     ====================================================================== */

  function uid() {
    return Date.now().toString(36) + Math.random().toString(36).slice(2, 8);
  }

  // Case-insensitive, whitespace-trimmed comparison key.
  function normalize(text) {
    return text.trim().toLowerCase();
  }

  function loadTasks() {
    try {
      const raw = localStorage.getItem(TASKS_KEY);
      if (!raw) return [];
      const data = JSON.parse(raw);
      if (!Array.isArray(data)) return [];
      return data
        .filter((t) => t && typeof t.text === 'string' && typeof t.completed === 'boolean')
        .map((t) => ({
          id: typeof t.id === 'string' && t.id ? t.id : uid(),
          text: t.text,
          completed: t.completed
        }));
    } catch (e) {
      return [];
    }
  }

  function saveTasks() {
    localStorage.setItem(TASKS_KEY, JSON.stringify(tasks));
  }

  function isDuplicate(text, excludeId) {
    const key = normalize(text);
    return tasks.some((t) => t.id !== excludeId && normalize(t.text) === key);
  }

  function showWarning(msg) {
    todoWarning.textContent = msg;
    todoWarning.hidden = false;
    clearTimeout(warningTimer);
    warningTimer = setTimeout(hideWarning, 3000);
  }

  function hideWarning() {
    todoWarning.hidden = true;
    clearTimeout(warningTimer);
  }

  function createTaskElement(task) {
    const li = document.createElement('li');
    li.className = 'todo-item' + (task.completed ? ' todo-item--done' : '');
    li.dataset.id = task.id;

    const check = document.createElement('input');
    check.type = 'checkbox';
    check.className = 'todo-item__check';
    check.checked = task.completed;
    check.setAttribute('aria-label', 'Mark task complete');

    const text = document.createElement('span');
    text.className = 'todo-item__text';
    text.textContent = task.text;

    const actions = document.createElement('div');
    actions.className = 'todo-item__actions';

    const editBtn = document.createElement('button');
    editBtn.type = 'button';
    editBtn.className = 'btn-icon';
    editBtn.dataset.action = 'edit';
    editBtn.setAttribute('aria-label', 'Edit task');
    editBtn.title = 'Edit';
    editBtn.innerHTML = ICON_EDIT;

    const delBtn = document.createElement('button');
    delBtn.type = 'button';
    delBtn.className = 'btn-icon btn-icon--danger';
    delBtn.dataset.action = 'delete';
    delBtn.setAttribute('aria-label', 'Delete task');
    delBtn.title = 'Delete';
    delBtn.innerHTML = ICON_DELETE;

    actions.appendChild(editBtn);
    actions.appendChild(delBtn);

    li.appendChild(check);
    li.appendChild(text);
    li.appendChild(actions);
    return li;
  }

  function renderTasks() {
    const sorted = tasks
      .slice()
      .sort((a, b) => Number(a.completed) - Number(b.completed));

    todoList.innerHTML = '';

    if (tasks.length === 0) {
      const empty = document.createElement('li');
      empty.className = 'empty-state';
      empty.textContent = 'No tasks yet — add one above.';
      todoList.appendChild(empty);
    } else {
      sorted.forEach((task) => todoList.appendChild(createTaskElement(task)));
    }

    updateStats();
  }

  function updateStats() {
    const total = tasks.length;
    const done = tasks.filter((t) => t.completed).length;
    const pending = total - done;

    statTotal.textContent = String(total);
    statDone.textContent = String(done);
    statPending.textContent = String(pending);
    todoCount.textContent = total === 1 ? '1 task' : total + ' tasks';
    todoStats.hidden = total === 0;
  }

  function addTask(text) {
    const trimmed = text.trim();
    if (!trimmed) {
      showWarning('Please enter a task.');
      return false;
    }
    if (isDuplicate(trimmed)) {
      showWarning('That task already exists.');
      return false;
    }
    tasks.push({ id: uid(), text: trimmed, completed: false });
    saveTasks();
    renderTasks();
    hideWarning();
    return true;
  }

  function toggleTask(id) {
    const task = tasks.find((t) => t.id === id);
    if (!task) return;
    task.completed = !task.completed;
    saveTasks();
    renderTasks();
  }

  function deleteTask(id) {
    tasks = tasks.filter((t) => t.id !== id);
    saveTasks();
    renderTasks();
  }

  function startEditTask(li) {
    if (!li || li.querySelector('.todo-edit-input')) return;
    const id = li.dataset.id;
    const task = tasks.find((t) => t.id === id);
    if (!task) return;

    const textEl = li.querySelector('.todo-item__text');

    const input = document.createElement('input');
    input.type = 'text';
    input.className = 'todo-edit-input';
    input.value = task.text;
    input.maxLength = 200;
    input.setAttribute('aria-label', 'Edit task');

    textEl.replaceWith(input);
    input.focus();
    input.select();

    let finished = false;

    function finish(commit) {
      if (finished) return;
      finished = true;
      const value = input.value.trim();
      if (commit && value && value !== task.text) {
        if (isDuplicate(value, id)) {
          showWarning('That task already exists.');
          renderTasks();
          return;
        }
        task.text = value;
        saveTasks();
      }
      hideWarning();
      renderTasks();
    }

    input.addEventListener('keydown', (e) => {
      if (e.key === 'Enter') { e.preventDefault(); finish(true); }
      else if (e.key === 'Escape') { e.preventDefault(); finish(false); }
    });
    input.addEventListener('blur', () => finish(true));
  }

  /* ======================================================================
     Phase D — Focus Timer
     ====================================================================== */

  function formatTimer(ms) {
    const total = Math.max(0, Math.ceil(ms / 1000));
    const m = Math.floor(total / 60);
    const s = total % 60;
    return pad(m) + ':' + pad(s);
  }

  function updateTimerDisplay(ms) {
    timerTime.textContent = formatTimer(ms);
  }

  function showTimerMessage(msg) {
    timerMessage.textContent = msg;
    timerMessage.hidden = false;
  }

  function hideTimerMessage() {
    timerMessage.hidden = true;
    timerMessage.textContent = '';
  }

  function ensureNotificationPermission() {
    if (!('Notification' in window)) return;
    if (Notification.permission === 'default') {
      try {
        const p = Notification.requestPermission();
        if (p && typeof p.catch === 'function') p.catch(function () {});
      } catch (e) { /* ignore */ }
    }
  }

  function notifyCompletion() {
    if (!('Notification' in window)) return;
    try {
      if (Notification.permission === 'granted') {
        new Notification('Focus session complete', { body: "Time's up! Great work." });
      }
    } catch (e) { /* ignore */ }
  }

  function updateTimerButtons() {
    const running = !!timerInterval;
    timerStart.disabled = running;
    timerPause.disabled = !running;
  }

  function tick() {
    const remaining = timerEndTime - Date.now();
    if (remaining <= 0) {
      timerRemaining = 0;
      updateTimerDisplay(0);
      completeTimer();
      return;
    }
    timerRemaining = remaining;
    updateTimerDisplay(remaining);
  }

  function startTimer() {
    if (timerInterval) return;                          // already running -> no second timer
    if (timerRemaining <= 0) timerRemaining = TIMER_DURATION; // fresh after completion
    timerEndTime = Date.now() + timerRemaining;
    timerDone = false;
    hideTimerMessage();
    ensureNotificationPermission();
    timerInterval = setInterval(tick, 1000);
    tick();
    updateTimerButtons();
  }

  function pauseTimer() {
    if (!timerInterval) return;
    timerRemaining = Math.max(0, timerEndTime - Date.now());
    clearInterval(timerInterval);
    timerInterval = null;
    updateTimerDisplay(timerRemaining);
    updateTimerButtons();
  }

  function resetTimer() {
    clearInterval(timerInterval);
    timerInterval = null;
    timerDone = false;
    timerRemaining = TIMER_DURATION;
    timerEndTime = null;
    hideTimerMessage();
    updateTimerDisplay(TIMER_DURATION);
    updateTimerButtons();
  }

  function completeTimer() {
    if (timerDone) return;
    timerDone = true;
    clearInterval(timerInterval);
    timerInterval = null;
    updateTimerButtons();
    showTimerMessage("Time's up! Your focus session is complete.");
    notifyCompletion();
  }

  /* ======================================================================
     Phase E — Quick Links
     ====================================================================== */

  // Add https:// if the user typed a bare domain. Leaves anything with a
  // scheme (e.g. "http://", "ftp://", "javascript:") untouched for validation.
  function normalizeUrl(input) {
    let s = input.trim();
    if (!s) return '';
    if (!s.includes('://')) s = 'https://' + s;
    return s;
  }

  // Returns { ok: true, url } or { ok: false, reason }.
  function validateUrl(input) {
    const s = normalizeUrl(input);
    if (!s) return { ok: false, reason: 'empty' };
    let url;
    try {
      url = new URL(s);
    } catch (e) {
      return { ok: false, reason: 'invalid' };
    }
    // Only http/https are allowed (blocks javascript:, data:, mailto:, etc.).
    if (url.protocol !== 'http:' && url.protocol !== 'https:') {
      return { ok: false, reason: 'unsafe' };
    }
    const host = url.hostname;
    if (!host) return { ok: false, reason: 'invalid' };
    if (host !== 'localhost' && !host.includes('.')) return { ok: false, reason: 'invalid' };
    return { ok: true, url: url.href };
  }

  function loadLinks() {
    try {
      const raw = localStorage.getItem(LINKS_KEY);
      if (!raw) return [];
      const data = JSON.parse(raw);
      if (!Array.isArray(data)) return [];
      const result = [];
      data.forEach((l) => {
        if (!l || typeof l.name !== 'string' || typeof l.url !== 'string') return;
        const v = validateUrl(l.url);
        if (!v.ok) return; // drop corrupted/unsafe stored urls
        result.push({
          id: typeof l.id === 'string' && l.id ? l.id : uid(),
          name: l.name.trim() || 'Link',
          url: v.url
        });
      });
      return result;
    } catch (e) {
      return [];
    }
  }

  function saveLinks() {
    localStorage.setItem(LINKS_KEY, JSON.stringify(links));
  }

  // Duplicate = same canonical URL as another link.
  function isDuplicateLink(url, excludeId) {
    return links.some((l) => l.id !== excludeId && l.url === url);
  }

  function showLinksWarning(msg) {
    linksWarning.textContent = msg;
    linksWarning.hidden = false;
    clearTimeout(linksWarningTimer);
    linksWarningTimer = setTimeout(hideLinksWarning, 3000);
  }

  function hideLinksWarning() {
    linksWarning.hidden = true;
    clearTimeout(linksWarningTimer);
  }

  function createLinkElement(link) {
    const li = document.createElement('li');
    li.className = 'link-item';
    li.dataset.id = link.id;

    const main = document.createElement('div');
    main.className = 'link-item__main';

    const a = document.createElement('a');
    a.className = 'link-item__link';
    a.textContent = link.name;
    a.href = link.url;
    a.target = '_blank';
    a.rel = 'noopener noreferrer';
    a.title = link.url;

    const urlSpan = document.createElement('span');
    urlSpan.className = 'link-item__url';
    urlSpan.textContent = link.url;

    main.appendChild(a);
    main.appendChild(urlSpan);

    const actions = document.createElement('div');
    actions.className = 'link-item__actions';

    const editBtn = document.createElement('button');
    editBtn.type = 'button';
    editBtn.className = 'btn-icon';
    editBtn.dataset.action = 'edit';
    editBtn.setAttribute('aria-label', 'Edit link');
    editBtn.title = 'Edit';
    editBtn.innerHTML = ICON_EDIT;

    const delBtn = document.createElement('button');
    delBtn.type = 'button';
    delBtn.className = 'btn-icon btn-icon--danger';
    delBtn.dataset.action = 'delete';
    delBtn.setAttribute('aria-label', 'Delete link');
    delBtn.title = 'Delete';
    delBtn.innerHTML = ICON_DELETE;

    actions.appendChild(editBtn);
    actions.appendChild(delBtn);

    li.appendChild(main);
    li.appendChild(actions);
    return li;
  }

  function renderLinks() {
    linksList.innerHTML = '';
    if (links.length === 0) {
      const empty = document.createElement('li');
      empty.className = 'empty-state';
      empty.textContent = 'No links yet — add your first shortcut above.';
      linksList.appendChild(empty);
    } else {
      links.forEach((link) => linksList.appendChild(createLinkElement(link)));
    }
  }

  function addLink(name, rawUrl) {
    const trimmedName = name.trim();
    const trimmedUrl = rawUrl.trim();
    if (!trimmedName) { showLinksWarning('Please enter a link name.'); return false; }
    if (!trimmedUrl) { showLinksWarning('Please enter a link address.'); return false; }
    const v = validateUrl(trimmedUrl);
    if (!v.ok) {
      showLinksWarning(
        v.reason === 'unsafe'
          ? 'Only http and https links are allowed.'
          : 'That does not look like a valid web address.'
      );
      return false;
    }
    if (isDuplicateLink(v.url)) { showLinksWarning('That link already exists.'); return false; }
    links.push({ id: uid(), name: trimmedName, url: v.url });
    saveLinks();
    renderLinks();
    hideLinksWarning();
    return true;
  }

  function deleteLink(id) {
    links = links.filter((l) => l.id !== id);
    saveLinks();
    hideLinksWarning();
    renderLinks();
  }

  // Replace the row with an inline edit form. Save/Enter commit, Cancel/Esc abort.
  function startEditLink(li) {
    if (!li || li.querySelector('form')) return; // not found or already editing
    const id = li.dataset.id;
    const link = links.find((l) => l.id === id);
    if (!link) return;

    li.className = 'link-item link-item--editing';
    li.innerHTML = '';

    const form = document.createElement('form');
    form.className = 'links-form';

    const nameInput = document.createElement('input');
    nameInput.type = 'text';
    nameInput.className = 'links-input';
    nameInput.value = link.name;
    nameInput.maxLength = 60;
    nameInput.setAttribute('aria-label', 'Link name');

    const urlInput = document.createElement('input');
    urlInput.type = 'text';
    urlInput.className = 'links-input';
    urlInput.value = link.url;
    urlInput.setAttribute('aria-label', 'Link address');

    const actions = document.createElement('div');
    actions.className = 'link-edit-actions';

    const saveBtn = document.createElement('button');
    saveBtn.type = 'submit';
    saveBtn.className = 'btn btn--accent';
    saveBtn.textContent = 'Save';

    const cancelBtn = document.createElement('button');
    cancelBtn.type = 'button';
    cancelBtn.className = 'btn btn--ghost';
    cancelBtn.textContent = 'Cancel';

    actions.appendChild(saveBtn);
    actions.appendChild(cancelBtn);

    form.appendChild(nameInput);
    form.appendChild(urlInput);
    form.appendChild(actions);
    li.appendChild(form);

    nameInput.focus();
    nameInput.select();

    form.addEventListener('submit', (e) => { e.preventDefault(); finishEdit(true); });
    cancelBtn.addEventListener('click', () => finishEdit(false));

    function onKey(e) {
      if (e.key === 'Escape') { e.preventDefault(); finishEdit(false); }
    }
    nameInput.addEventListener('keydown', onKey);
    urlInput.addEventListener('keydown', onKey);

    function finishEdit(commit) {
      if (commit) {
        const name = nameInput.value.trim();
        const url = urlInput.value.trim();
        if (!name) { showLinksWarning('Please enter a link name.'); nameInput.focus(); return; }
        if (!url) { showLinksWarning('Please enter a link address.'); urlInput.focus(); return; }
        const v = validateUrl(url);
        if (!v.ok) {
          showLinksWarning(
            v.reason === 'unsafe'
              ? 'Only http and https links are allowed.'
              : 'That does not look like a valid web address.'
          );
          urlInput.focus();
          return;
        }
        if (isDuplicateLink(v.url, id)) { showLinksWarning('That link already exists.'); urlInput.focus(); return; }
        link.name = name;
        link.url = v.url;
        saveLinks();
      }
      hideLinksWarning();
      renderLinks();
    }
  }

  /* ======================================================================
     Phase F — Light / Dark theme
     ====================================================================== */

  function loadTheme() {
    const stored = localStorage.getItem(THEME_KEY);
    return stored === 'light' || stored === 'dark' ? stored : 'dark';
  }

  function applyTheme(theme) {
    const t = theme === 'light' ? 'light' : 'dark';
    document.documentElement.setAttribute('data-theme', t);
    if (themeIcon) themeIcon.textContent = t === 'light' ? '☀️' : '🌙';
    themeToggle.setAttribute('aria-label', t === 'light' ? 'Switch to dark mode' : 'Switch to light mode');
    themeToggle.title = t === 'light' ? 'Switch to dark mode' : 'Switch to light mode';
  }

  function toggleTheme() {
    const current = document.documentElement.getAttribute('data-theme') === 'light' ? 'light' : 'dark';
    const next = current === 'light' ? 'dark' : 'light';
    applyTheme(next);
    localStorage.setItem(THEME_KEY, next);
  }

  /* ---------- Init ---------- */
  function init() {
    // Name display + editing.
    renderName();
    nameEl.style.cursor = 'pointer';
    nameEl.title = 'Click to edit your name';
    nameEl.addEventListener('click', startNameEdit);

    // Clock + greeting, update every second.
    updateClockAndGreeting();
    setInterval(updateClockAndGreeting, 1000);

    // To-do list.
    renderTasks();

    todoForm.addEventListener('submit', (e) => {
      e.preventDefault();
      if (addTask(todoInput.value)) {
        todoInput.value = '';
        todoInput.focus();
      }
    });

    todoList.addEventListener('change', (e) => {
      if (e.target.classList.contains('todo-item__check')) {
        const li = e.target.closest('.todo-item');
        if (li) toggleTask(li.dataset.id);
      }
    });

    todoList.addEventListener('click', (e) => {
      const btn = e.target.closest('[data-action]');
      if (!btn) return;
      const li = btn.closest('.todo-item');
      if (!li) return;
      if (btn.dataset.action === 'edit') startEditTask(li);
      else if (btn.dataset.action === 'delete') deleteTask(li.dataset.id);
    });

    // Focus timer.
    updateTimerDisplay(timerRemaining);
    updateTimerButtons();
    timerStart.addEventListener('click', startTimer);
    timerPause.addEventListener('click', pauseTimer);
    timerReset.addEventListener('click', resetTimer);

    // Quick links.
    renderLinks();

    linksForm.addEventListener('submit', (e) => {
      e.preventDefault();
      if (addLink(linkNameInput.value, linkUrlInput.value)) {
        linkNameInput.value = '';
        linkUrlInput.value = '';
        linkNameInput.focus();
      }
    });

    linksList.addEventListener('click', (e) => {
      const btn = e.target.closest('[data-action]');
      if (!btn) return;
      const li = btn.closest('.link-item');
      if (!li) return;
      if (btn.dataset.action === 'edit') startEditLink(li);
      else if (btn.dataset.action === 'delete') deleteLink(li.dataset.id);
    });

    // Light / dark theme.
    applyTheme(loadTheme());
    themeToggle.addEventListener('click', toggleTheme);
  }

  if (document.readyState === 'loading') {
    document.addEventListener('DOMContentLoaded', init);
  } else {
    init();
  }
})();
