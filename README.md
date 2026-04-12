# 📅 Calendário Pai & Mãe

A mobile-first, single-file web calendar that displays a rotating custody schedule between two guardians ("Pai" and "Mãe"), with Portuguese national holidays, month/year navigation, and swipe gesture support.

**Live demo:** [ricgon-apps.github.io/calendario](https://ricgon-apps.github.io/calendario)

---

## Overview

This project solves a practical co-parenting problem: quickly knowing whose custody day it is on any given date. The schedule follows a fixed 14-day repeating cycle, displayed in a clean calendar view with colour-coded labels for each guardian.

The app runs entirely in the browser — no server, no framework, no dependencies to install. One HTML file is all it takes.

**Target users:** Co-parents and family members who need a shared, always-accessible reference for a custody schedule.

---

## Features

- 🟢 **Colour-coded custody tags** — Mãe (green) and Pai (dark grey) displayed per day
- 🔁 **14-day repeating cycle** — automatically calculated from a fixed reference date
- 🇵🇹 **Portuguese national holidays** — fixed and variable (Easter-based) holidays marked with a flag; tap a holiday day to see its name
- 📆 **Month/Year picker** — tap the month title to jump directly to any month and year
- ◀ ▶ **Navigation buttons** — step through months one at a time
- 👆 **Swipe gestures** — swipe left/right on the calendar to change month (touch devices)
- 📱 **Mobile-first design** — optimised for iOS Safari and Android browsers
- ⚡ **Zero dependencies** — pure HTML, CSS, and vanilla JavaScript; works offline after first load
- 🗓️ **Today indicator** — current day highlighted automatically

---

## Tech Stack

| Layer | Technology |
|---|---|
| Markup | HTML5 |
| Styling | CSS3 (custom properties, grid, keyframe animations) |
| Logic | Vanilla JavaScript (ES5-compatible) |
| Fonts | Google Fonts — DM Sans + DM Serif Display |
| Hosting | GitHub Pages |

No build tools, no bundler, no package manager.

---

## How It Works

### Custody Cycle

The schedule is based on a **14-day cycle** anchored to a known reference date (`April 26, 2026`). For any given date, the number of days elapsed since the reference is computed, and the result modulo 14 maps to a slot in the cycle array. Each slot defines which guardian(s) appear that day and in what order.

```
cycle index = ((daysDiff % 14) + 14) % 14
```

The cycle array encodes 14 patterns, for example:

```js
var CYCLE = [
  ['mae'],        // index 0
  ['mae','pai'],  // index 1
  ['pai'],        // index 2
  ...
];
```

### Holiday Detection

**Fixed holidays** are stored as a simple key-value object keyed by `month-day`.

**Variable holidays** (Good Friday, Easter Sunday, Corpus Christi) are computed dynamically using the [Meeus/Jones/Butcher algorithm](https://en.wikipedia.org/wiki/Computus#Anonymous_Gregorian_algorithm) for any given year.

### Swipe Gestures

Swipe support uses `touchstart` and `touchend` events with `passive: true` — compatible with iOS Safari's strict scroll-event restrictions. A horizontal swipe of at least 40px triggers a month change.

---

## Getting Started

### Prerequisites

None. You only need a modern web browser.

### Run Locally

```bash
# Clone the repository
git clone https://github.com/ricgon-apps/calendario.git

# Open in browser
open index.html
# or simply double-click index.html
```

No installation, no build step.

### Deploy to GitHub Pages

1. Go to **Settings → Pages** in your repository
2. Under **Branch**, select `main` and folder `/ (root)`
3. Click **Save**
4. Your calendar will be live at `https://<your-username>.github.io/calendario`

---

## Usage

| Action | How |
|---|---|
| Next month | Tap **›** button or swipe left |
| Previous month | Tap **‹** button or swipe right |
| Jump to month/year | Tap the **month title** (e.g. "Abril 2026 ▾") |
| See holiday name | Tap any day marked with 🇵🇹 |

---

## Folder Structure

```
calendario/
├── index.html   # Entire application — markup, styles, and logic
└── README.md    # Project documentation
```

The project intentionally lives in a single file for maximum portability. It can be saved locally, shared via messaging apps, or hosted on any static file server.

---

## Configuration

To adapt the custody cycle to a different schedule, edit two constants inside `index.html`:

### 1. Reference date

```js
var REFERENCE_DATE = new Date(2026, 3, 26); // month is 0-indexed (3 = April)
```

Set this to a date where you know the cycle position with certainty.

### 2. Cycle array

```js
var CYCLE = [
  ['mae'],        // day 0 relative to reference
  ['mae','pai'],  // day 1
  ['pai'],        // day 2
  // ... 14 entries total
];
```

Each entry is an array of guardian identifiers. The order of items determines the visual stacking order (first = top). Supported values: `'mae'`, `'pai'`.

### Adding or renaming guardians

1. Add a new CSS class in the `<style>` block:
   ```css
   .tag.outro { background: #e67e22; color: #fff; }
   ```
2. Add the identifier to the relevant cycle slots
3. Update the display label in `makeCell()`:
   ```js
   t.textContent = tags[i] === 'outro' ? 'Outro' : ...;
   ```

---

## Portuguese National Holidays Included

| Date | Holiday |
|---|---|
| 1 January | Ano Novo |
| Variable (Friday before Easter) | Sexta-feira Santa |
| Variable (Easter Sunday) | Páscoa |
| 25 April | Dia da Liberdade |
| 1 May | Dia do Trabalhador |
| Variable (+60 days after Easter) | Corpo de Deus |
| 10 June | Dia de Portugal |
| 15 August | Assunção de Nossa Senhora |
| 5 October | Implantação da República |
| 1 November | Dia de Todos os Santos |
| 1 December | Restauração da Independência |
| 8 December | Imaculada Conceição |
| 25 December | Natal |

> Municipal holidays (e.g. Santo António in Lisbon, São João in Porto) are not included, as they vary by location.

---

## Roadmap / Future Improvements

- [ ] **PWA support** — add a `manifest.json` and service worker so the app can be installed on the home screen and used fully offline
- [ ] **Municipal holidays** — allow the user to select their municipality to include local holidays
- [ ] **Configurable cycle** — a settings panel to let users define their own custody pattern without editing code
- [ ] **iCal export** — generate a `.ics` file to import the schedule into Apple Calendar or Google Calendar
- [ ] **Multi-language support** — English and Spanish localisations
- [ ] **Accessibility** — improve keyboard navigation and ARIA labels

---

## Known Limitations

- The custody cycle is **hardcoded** — changing the schedule requires editing the source file directly
- Swipe gestures work on touch devices; **mouse drag is not supported** (by design, to avoid conflicts with iOS Safari scroll restrictions)
- The app **requires an internet connection on first load** to fetch Google Fonts; layout still works offline but will fall back to system fonts
- The `id="holidayBanner"` text may appear in the header when the file is **previewed inside an iframe** (e.g. Claude.ai artifact viewer) — this is a sandboxing artefact and does not appear in normal browser usage

---

## Contributing

Contributions are welcome. To propose a change:

1. Fork the repository
2. Create a branch: `git checkout -b feature/your-feature`
3. Make your changes to `index.html`
4. Open a Pull Request with a clear description of what was changed and why

Since the entire project is a single HTML file, there is no build process to worry about. Test your changes by opening `index.html` directly in a browser and on a mobile device (or using browser DevTools device emulation).

---

## License

This project is licensed under the **MIT License** — you are free to use, copy, modify, and distribute it, including for commercial purposes, as long as the original copyright notice is retained.

---

## Author

**ricgon-apps** — [github.com/ricgon-apps](https://github.com/ricgon-apps)
