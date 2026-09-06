# RoshanSystem — v19 snapshot

[![Python](https://img.shields.io/badge/Python-PySide6-3776ab?logo=python&logoColor=white)](Roshan%20System%20-%20Python%20Version)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Status](https://img.shields.io/badge/Status-Experimental-orange.svg)]()
[![Web Apps](https://img.shields.io/badge/HTML-Web%20Apps-orange?logo=html5&logoColor=white)](Roshan%20System%20-%20Python%20Version)
[![Web Apps](https://img.shields.io/badge/CSS-Web%20Apps-blue?logo=css&logoColor=white)](Roshan%20System%20-%20Python%20Version)
[![Web Apps](https://img.shields.io/badge/Javascript-Web%20Apps-yellow?logo=javascript&logoColor=white)](Roshan%20System%20-%20Python%20Version)


#### WARNING — Early v19 snapshot
This repository contains an early, experimental snapshot of RoshanSystem v19. Many v19 features are still being implemented and refactored, on-disk formats and APIs may change, and there are known rough edges. This snapshot is intended for development and experimentation — not production use.

## What this is
RoshanSystem is a Python-based desktop-environment simulation (GUI shell + bundled apps) built with PySide6 (Qt). It provides a desktop shell with a background, a taskbar/Start menu, login screen, and several built-in applications (Notepad, Calculator, File Explorer, Image Viewer, Terminal, Paint, Run, Rosver). The project is educational and experimental.

#### Key highlights from the current refactor
- GUI now uses PySide6 (Qt); main entrypoint: `Roshan System - Python Version/main.py`.
- A `core/` package centralizes reusable components:
  - `core/Window.py` — Window and WebWindow base classes.
  - `core/filedialog.py` — Custom open/save file dialogs.
  - `core/styling.py` — Loads QSS style files via `core.get_qss_styles()`.
- Apps are in `Roshan System - Python Version/apps/` and are dynamically loaded from `apps.json`.
- Run apps are listed in `run_apps/run_apps.json` (e.g., `rosver` shows the v19 version).
- Styling is driven by QSS files under `styling/`; QSS is loaded per-directory.
- Settings are persisted in `settings.json`. Login details are stored in `login_details.json` (SHA3-512 hashed).

## Quick start (shortest path)                         
Prerequisites: Python 3.10+ recommended.

Install and run:
```bash
cd "Roshan System - Python Version"
pip install -e .
python3 main.py
```

or:
```bash
cd "Roshan System - Python Version"
python3 main.py
```

or if you are on windows:
```bash
cd "Roshan System - Python Version"
py main.py
```

On first launch you create a local account (username + password). Subsequent launches show the login form.

Project layout (relevant)
```
Roshan System - Python Version/
  main.py                 # Desktop shell, app loader, taskbar, Start menu
  login_page.py           # Local sign-up/login (SHA3-512)
  apps.json               # Taskbar/Start menu registry (dynamic import)
  requirements.txt        # PySide6
  line_counter.py         # small utility
  apps/                   # bundled apps (notepad.py, calculator.py, fileexplorer.py, imageviewer.py, paint.py, terminal.py, run.py ...)
  run_apps/               # run-app registry and small tools (rosver.py)
  core/                   # Window, WebWindow, filedialog, styling loader
  styling/                # QSS files organized by component
  textures/               # images (icons, backgrounds)
  user_dir/               # created at runtime for user files
```

#### How it fits together
- `main.py` reads `settings.json` (or writes defaults), loads QSS via `core.get_qss_styles()`, reads `apps.json`, dynamically imports apps and instantiates them, and composes the desktop (background, taskbar, Start menu).
- Apps are Window-like objects (based on `core.Window`) and are shown/hidden and moved by the desktop.
- `WebWindow` embeds HTML via Qt WebEngine and delegates file selection to `core.filedialog`.

#### Security & important warnings
- The Terminal runs host shell commands. There is no sandboxing — do not run untrusted commands.
- Login is local-only: credentials are hashed and stored locally; this is not a secure multi-user authentication system.
- This is experimental code: do not use on machines containing sensitive data.

#### Known issues & refactor observations (concrete)
- README previously referenced CustomTkinter/Pillow — outdated; code now uses PySide6. README updated to reflect this.
- Typo: `settings.json` default uses the key `maximized`, but `main.py` checks `settings["maxmimized"]` when deciding to call `showMaximized()` — this typo can break expected behaviour.
- Dynamic import pattern in `main.py` uses `__import__` + `getattr`; when adding apps ensure module/class names match.
- Styling relies on `.qss` files in `styling/` subdirectories; ensure those directories contain `.qss` files for the styles to load.
- `core.WebEnginePage.chooseFiles` uses `core.filedialog` dialogs when web pages request file selection.

## Roadmap / immediate TODOs before official v19
- Finish and stabilize v19 feature set.
- Fix the `maximized`/`maxmimized` typo and validate settings keys at startup.
- Improve Terminal safety (sandbox, confirmations, permission model).
- Add packaging/installer instructions and CI/testing for UI and core logic.
- Update and finish docs to match the refactor.

## Contributing
- Contributions are welcome. See CONTRIBUTING.md and CODE_OF_CONDUCT.md.
- Good first PRs:
  - Fix the `maximized` typo and add a runtime validation or unit to prevent regressions.
  - Update any remaining CustomTkinter references.
  - Add safer Terminal behavior or confirmation prompts.
  - Improve `core.get_qss_styles()` error handling for missing or empty QSS dirs.

## License
- MIT (see LICENSE)

## Acknowledgements
- This snapshot was refactored into a modular core and PySide6-based GUI; thank you to all contributors listed in `run_apps/rosver.py`.
