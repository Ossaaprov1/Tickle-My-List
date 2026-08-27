# Tickle my List — Desktop v0.6.3

A local-first Windows desktop project/checklist application with reusable checklist templates, project-specific items, an always-available overlay, custom palettes, smooth drag-and-drop, and local safety/export tools.


## v0.6.3 — Scoped refresh + Lock-only proportional overlay

### Local page refresh

Routine data changes no longer rebuild the whole visible application. The main shell/navigation and current page surface stay alive while only the affected region is refreshed. Hidden pages are marked dirty and update when you navigate to them instead of rebuilding in the background.

Examples:

- ticking an item in the overlay refreshes the overlay immediately
- if the full app is showing the affected project, its visible project/active region updates immediately
- unrelated pages such as Settings are left untouched
- theme/palette changes still use the intentional full-UI rebuild path because every widget colour changes

### Lock replaces Pin

Pin has been removed from the live overlay and Settings. **Lock** is now the single movement/topmost state:

- **Unlocked** — movable, corner-resizable, Snap available, not forced always-on-top
- **Locked** — immovable, non-resizable, always-on-top above ordinary desktop windows, with only the Unlock icon visible

When locked, Snap, Minimise, Open Full App, Exit, and resize handles are hidden/disabled. Existing legacy `overlay_pinned` values in older local databases are ignored rather than deleting user settings.

### Proportional corner scaling

The expanded overlay can be scaled from any corner while unlocked. Corner dragging preserves the overlay aspect ratio and scales the complete interface together: window, fonts, icons, checkboxes, progress ring, cards, padding, spacing, and scroll content.

The supported scale range is **50%–200%**. The final scale persists across launches and remains editable from Settings.

If **Snap** is enabled while resizing, the overlay stays attached to its current screen edge for the entire resize. The v0.6.2 edge-aware opacity fade remains and continues to orient itself from the closest screen edge toward the desktop centre.


## v0.6.2 — Edge-aware overlay controls + perimeter snap

### Edge-aware overlay fade

The floating overlay now uses an edge-oriented surface fade tied to its opacity setting. The side nearest the screen border stays visually strongest and the surface fades inward toward the centre of the desktop. The direction updates automatically for the left, right, top, and bottom screen edges while the existing Windows alpha setting still controls the overall overlay opacity.

### Header movement and lock/pin behaviour

The logo/project-title portion of the expanded overlay is now the dedicated drag surface; the control-button cluster is excluded. The overlay can only be dragged when it is both **unlocked** and **unpinned**.

When locked:

- the overlay position cannot be dragged
- Pin is hidden
- Minimise is hidden
- Open Full App is hidden
- Exit is hidden
- Snap remains visible
- Unlock remains visible

Lock, Pin, Exit, Snap, and Minimise use compact monochrome icon controls rather than text labels.

### Perimeter Snap

A new **Snap** icon sits beside Pin. When Snap is enabled and the overlay is movable, the overlay is projected onto the screen perimeter while dragging: the mouse chooses the nearest screen edge, and the overlay slides along that edge while preserving the pointer offset. Moving around a corner naturally transfers the overlay to the next edge. Snap state and the final overlay position persist locally.


## v0.6.1 — Project details + calendar dates + redesigned reports

### Optional project details

New projects now start with **no workflow status**. Status, Project Due Date, Reminder, and Tags are hidden from the Project page until they are actually assigned. Use **+ Project Details** to add or change them. Removing the last optional detail collapses the section back to the single `+ Project Details` control.

Existing v0.6.0 project statuses are preserved during migration because the app cannot safely tell whether an existing `Upcoming` value was automatic or manually selected. New projects are blank by default.

### Calendar dates

Project Due Date and Reminder now use a styled in-app calendar selector. Both are date-only and display as `DD-MM-YYYY`. Dates remain stored internally as ISO values so sorting, due-state calculations, and local reminders stay reliable.

Automatic due labels are:

- past due — **Overdue**
- due today — **Due Today**
- one day away — **Upcoming**
- more than one day away — no automatic due label

Manual workflow status remains separate and is only shown when assigned.

### Redesigned PDF export

The ReportLab PDF renderer now uses the same restrained visual language as the desktop app while keeping the report print-friendly:

- white A4 background
- current themed Tickle my List logo and Logo palette colour
- branded header/rule
- circular completion summary
- optional status/due/reminder metadata only when assigned
- coloured tag pills
- rounded notes/checklist/item sections
- vector completed/incomplete checkboxes
- wrapped long text and multi-page continuation headers
- `DD-MM-YYYY` report dates

CSV and plain-text exports also use `DD-MM-YYYY` and no longer print empty optional metadata as `Not set`.

### Window background behaviour

The main window and app-owned dialogs are opaque again and fill to the Windows window border. The floating overlay remains the one exception and keeps its transparent rounded corners.

## v0.6.0 — Project management + safety tools

### Project notes, tags, status and dates

Projects now support:

- project notes
- coloured project tags
- workflow status: **Upcoming, Active, Waiting, Complete**
- project due date
- reminder date/time

Due-date badges are derived automatically when relevant: **Due Today**, **Tomorrow**, and **Overdue**. A project marked Complete stays Complete.

### Windows reminders

A reminder can be set for a project using `YYYY-MM-DD HH:MM`. When the reminder becomes due, Tickle my List sends a local Windows notification while the app is running (including when it is minimised to the tray). Reminders do not use a cloud service.

### Archive and Recycle Bin

**Archive** is now the primary way to remove a project from the active workspace.

- Archived projects have their own searchable Archive page.
- Archived projects can be restored.
- Delete remains a secondary action and sends the project to the Recycle Bin.
- Deleted projects and reusable checklist templates remain in the local Recycle Bin for **30 days**.
- Recycle Bin items can be restored or permanently deleted.
- Items older than 30 days are automatically purged locally.

### Export and Print

Project view now supports:

- **PDF** checklist/completion report
- **CSV** export
- **Plain text** export
- **Print** using a generated printable PDF

Exports include the project name, workflow/due status, due date, completion percentage, tags, report date, notes, checklist names, checklist items, and completion states.

### Keyboard controls

The requested desktop shortcuts are included and can be changed in **Settings → Keyboard Controls**:

- `Ctrl + N` — new project
- `Ctrl + Shift + N` — new checklist
- `/` — search
- `Space` — check/uncheck selected project checklist item
- `↑ / ↓` — navigate checklist items
- `Ctrl + D` — duplicate selected project/checklist
- `Ctrl + Z` — undo supported recent actions

Press **Change** beside a shortcut in Settings, then press the replacement key combination. Conflicting shortcuts are rejected.

### Overlay controls

Settings now also includes:

- proportional overlay scale from **50% to 200%**
- **opacity** from 35% to 100%
- **Lock / Unlock** movement + always-on-top behaviour

While unlocked, the expanded overlay exposes Snap, Lock, Minimise, Open Full App, Exit, and four corner resize zones. When locked, only Unlock remains visible.

## Existing features retained

- reusable checklist templates
- project-only checklist items
- independent checklist copies per project
- circular animated progress
- smooth drag/drop ordering
- Light / Dark / Custom editable palettes
- editable logo colour
- system tray
- `Ctrl + Alt + L` overlay shortcut
- local SQLite autosave
- daily local database backups
- no account, subscription, or server required

## Windows setup

### Build the standalone EXE

Double-click:

`build_windows_exe.bat`

The build script locates/installs Python when needed, installs the desktop/PDF/notification dependencies, then creates:

`dist\TickleMyList.exe`

### Run from source

Double-click:

`run_windows.bat`

### Debug

Double-click:

`run_debug_windows.bat`

## Local data

Windows application data is stored at:

`%APPDATA%\TickleMyList`

The database is `job_checklist.db`; daily backups are stored in the adjacent `backups` folder. Existing v0.5.x databases are upgraded in place without rewriting project/checklist content.
