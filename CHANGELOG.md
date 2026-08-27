# Changelog

## v0.6.3 — Scoped refresh + Lock-only proportional overlay

- Added semantic page/region refresh routing so routine project/checklist changes no longer rebuild the whole current application surface.
- Hidden pages now keep dirty refresh scopes and update when opened instead of rebuilding in the background.
- Overlay checklist changes refresh the overlay immediately and only update visible full-app regions that depend on the affected project.
- Kept global theme/palette changes on the intentional full-UI rebuild path.
- Removed the live Pin control and ignored legacy `overlay_pinned` values for behaviour.
- Lock is now the single overlay movement/topmost state: unlocked is movable/resizable and not forced topmost; locked is fixed, non-resizable, and always-on-top.
- Locked overlay state now hides Snap, Minimise, Open Full App, Exit, and corner resize affordances, leaving only Unlock.
- Added four-corner proportional overlay resizing while unlocked.
- Corner resizing preserves aspect ratio and scales the complete overlay UI, including fonts, icons, checkboxes, progress, cards, padding, and spacing.
- Expanded the saved overlay scale range to **50%–200%** and updated Settings slider, exact percentage entry, and quick presets.
- When Snap is active, proportional resize keeps the overlay attached to its current screen edge throughout the drag.
- Preserved v0.6.2 perimeter Snap and edge-aware opacity-gradient behaviour.
- Added v0.6.3 regression coverage for Lock-only state/topmost logic, proportional corner geometry, snapped-edge anchoring, scoped page refresh identity, and mutation routing.

## v0.6.2 — Edge-aware overlay + perimeter snapping

- Added a persistent overlay **Snap** state and icon control beside Pin.
- Added nearest-edge/perimeter drag projection so snapped overlays can only move around the outer screen border.
- Snap dragging preserves the grab offset along the active edge and transitions between edges based on mouse proximity.
- Added edge-aware overlay surface fading: the screen-edge side remains strongest and fades inward; orientation follows the nearest left/right/top/bottom edge.
- Kept the existing Windows whole-window opacity setting and tied the visual fade strength to the same opacity percentage.
- Changed the expanded overlay header into a dedicated left-side drag zone, excluding the control-button area.
- Overlay dragging is now disabled whenever the overlay is either **Locked** or **Pinned**.
- Locked overlays hide Pin, Minimise, Open Full App, and Exit while keeping Snap and Unlock available.
- Replaced Lock, Pin, Exit, Snap, and Minimise text controls with compact monochrome icon buttons.
- Snap state and final overlay position persist in the existing local settings table.
- Added pure geometry/state regression tests for edge selection, perimeter snapping, drag eligibility, lock visibility, and gradient direction/strength.
- Added a headless expanded-overlay smoke test across all four screen edges, Lock, Pin, Snap, and opacity states.

## v0.6.1 — Calendar metadata + app-styled PDF reports

- New projects now store a blank workflow status by default; status only appears after being explicitly assigned.
- Preserved existing v0.6.0 status values during migration instead of guessing which were manually assigned.
- Added a styled in-app calendar selector for Project Due Date and Reminder.
- Changed reminder behavior from date/time to date-only.
- Changed all new user-facing project/export date formatting to `DD-MM-YYYY` while preserving ISO storage internally.
- Changed automatic due labels to **Overdue**, **Due Today**, and **Upcoming** when exactly one day away; no automatic due label appears more than one day ahead.
- Added a `+ Project Details` popup for Status, Project Due Date, Reminder, and Tags.
- Project Status, Due Date, Reminder, and Tags now disappear from the Project page when unset.
- Rebuilt ReportLab PDF export into an app-styled A4 report with the current themed logo, brand rule, circular progress, optional metadata, coloured tag pills, notes, rounded checklist rows, vector checkboxes, wrapping, pagination, and page footers.
- PDF, CSV, and plain-text exports now omit unset optional project metadata.
- PDF, CSV, and plain-text export dates now use `DD-MM-YYYY`.
- The main app window and app-owned dialogs now fill to the normal Windows border; transparent colour-key corners are opt-in and retained only for the floating overlay.
- Local reminder checks now use date-only comparison and continue to support legacy datetime reminder values through their date portion.
- Added v0.6.1 automated coverage for date/status semantics, blank-status project creation, date-only reminders, conditional metadata, calendar grid generation, opaque-vs-overlay transparency, and PDF/TXT/CSV exports.
- Verified a representative PDF and a five-page stress report with the PDF render workflow; no clipping/overlap was observed.

## v0.6.0 — Project metadata, archive safety, export/print, reminders and shortcuts

- Added per-project notes.
- Added per-project coloured tags with add/edit/delete controls.
- Added workflow statuses: **Upcoming**, **Active**, **Waiting**, and **Complete**.
- Added project due dates with automatic **Due Today**, **Tomorrow**, and **Overdue** display states.
- Added project reminder date/time and local Windows notification delivery while the app is running/in the tray.
- Added an Archive workflow as the primary removal action for projects.
- Added a searchable Archive page with Restore and secondary Delete-to-Bin actions.
- Added a local Recycle Bin for deleted projects and reusable checklist templates.
- Added Restore, Delete Forever, and automatic 30-day purge behavior.
- Added PDF project/completion reports using ReportLab.
- Added CSV and plain-text project exports.
- Added direct Print using the generated project PDF and the Windows print verb, with file-open fallback.
- Added requested keyboard controls: Ctrl+N, Ctrl+Shift+N, /, Space, arrow navigation, Ctrl+D, and Ctrl+Z.
- Added a Settings keyboard-control editor that captures a replacement key combination and prevents conflicts.
- Added a styled global search covering active projects, checklist templates, and archived projects.
- Added local undo coverage for core create/duplicate/archive/delete/reorder/check/metadata operations.
- Added overlay opacity control from 35% to 100%.
- Added overlay Pin/Unpin always-on-top control.
- Added overlay Lock/Move position control.
- Added Pin/Unpin and Lock/Move buttons directly to the expanded overlay.
- Added non-destructive v0.6 SQLite migration fields/tables for notes, dates, reminder state, workflow status, archive/delete timestamps, and tags.
- Preserved project-only checklist items, themes/palettes, logo colour, drag/drop, circular progress, overlay scaling, tray behavior, and existing v0.5.x data.
- Fixed PDF export to accept path-like values as well as normal string paths.
- Regression coverage passes for migration, due-date labels, reminders query/fallback notification path, tag persistence, archive/search/restore, recycle/restore/purge, duplicate-project metadata, all primary UI pages, overlay scale/opacity/pin/lock, shortcut parsing, and TXT/CSV/PDF exports.

## v0.5.7 — Overlay area fit + project-only items + logo palette

- Changed the existing overlay percentage control to scale the overlay **area** first rather than scaling every internal element at exactly the same ratio.
- Added adaptive overlay content fitting so text and controls remain in a readable size range from 75% to 150%.
- Content fit now respects actual screen-constrained overlay dimensions when Windows cannot provide the full requested area.
- Long overlay checklist content remains inside the rounded window and uses the existing internal scroll view instead of overflowing the panel.
- Added **+ Add Project Item** to the selected checklist in Project view.
- Project-added items are stored only in the assigned project checklist instance and never modify the reusable template.
- Project-added items participate in completion, progress rings, Active overlay display, and existing checklist drag/reorder behaviour.
- Added a tenth editable palette token: **Logo**.
- Added logo recolouring that changes the coloured logo body while preserving the white checklist glyph.
- Logo palette changes apply to the main navigation logo, overlay, dialogs, runtime window icon, and Windows tray icon.
- Existing Light/Dark/Custom/saved palettes without a Logo value automatically inherit the original blue logo colour.
- Existing project/checklist database schema remains compatible; only the existing settings and job checklist item tables are used.
- Regression tests pass for project-only item isolation, progress inclusion, legacy palette fallback, overlay construction at 75/100/150%, Settings, and runtime logo recolouring.

## v0.5.6 — Scroll continuity + hover polish + smoother refresh

- Removed the heavy/dark-looking outline from the lifted drag state; drag feedback now relies on the existing tint, shadow, placeholder and motion.
- Reworked ScrollFrame wheel routing so the mouse wheel keeps scrolling while the pointer is over cards, labels, buttons, checkboxes, entries, and other child controls.
- Added descendant wheel binding for dynamically created scroll content, with duplicate-binding protection and coalesced rebinding during page construction.
- Added a subtle grow/lift hover treatment to rounded buttons without changing surrounding layout geometry.
- Added subtle grow/lift hover feedback to clickable and draggable rounded cards.
- Added hover growth/feedback to checkboxes, rounded fields, overlay-size slider thumb, scrollbar thumb, checklist drag handles, and the clickable app logo.
- Removed the old draggable-card hover outline so hover feedback is consistent with the global grow/lift treatment.
- Added coalesced page refresh scheduling so repeated updates from one interaction produce one page rebuild instead of redundant redraws.
- Added coalesced overlay refresh scheduling for the same reason.
- Cached stable text fonts and reduced repeated font measurement in common helper functions.
- Kept data/schema compatibility with existing v0.5.5 databases.
- Added regression coverage for scroll-wheel routing, hover bindings, coalesced page refresh, coalesced overlay refresh, and all main UI views.

## v0.5.5 — Smooth drag + animated circular progress

- Checklist drag uses a dedicated handle so typing/editing or ticking a checkbox cannot accidentally start a drag.
- Rebuilt the global card drag engine around floating absolute positions during a drag.
- Grabbed cards now follow the pointer directly while neighbouring cards smoothly lerp into the proposed insertion gap.
- Drop targets update without queuing stale animations, keeping rapid direction changes responsive.
- On release, the grabbed card eases into the exact destination slot before the new order is committed.
- The same smooth motion engine now applies to Home project blocks, Projects list cards, checklist-template cards, template checklist items, and project checklist items.
- Existing up/down checklist-item controls remain available.
- Rebuilt circular progress ring geometry so stroke thickness is consistent and round end caps sit on the actual stroke centre-line.
- Removed the protruding/bulged arc-cap artefact visible at partial percentages.
- Circular progress values now interpolate with an ease-out curve between previous and new completion percentages.
- Progress interpolation state is retained across view rebuilds so checking/unchecking a project item animates from the previous percentage instead of restarting from zero.
- Animated circular progress is applied to Home, Projects list, Project Overall Progress, and the floating overlay.
- No new project/checklist data fields were added; existing v0.5.3 databases remain compatible.
- Regression tests pass for template-item reorder persistence, project-checklist reorder persistence, smooth drag target placement, progress interpolation, and all main views.

## v0.5.3 — Smooth rendering + palette stability

- Replaced raw Tk rounded-corner drawing with supersampled anti-aliased Pillow rendering for smoother radius edges.
- Applied the smoother renderer to cards, buttons, badges, entries, checkboxes, progress bars, scrollbars, sliders, panels, dialog surfaces, and other rounded app-owned controls.
- Reworked circular progress rings with supersampled anti-aliased arcs and rounded arc ends.
- Added Windows per-monitor DPI awareness before Tk starts to reduce bitmap-scaling blur on high-DPI displays.
- Removed root-window alpha fading during theme changes; palette application now stays at full opacity.
- Added coalesced/debounced theme rebuilding so rapid palette selections result in one redraw rather than multiple synchronous refreshes.
- Cached anti-aliased shape assets between theme switches to reduce palette-redraw work.
- Made the palette colour dialog non-modal so the main application no longer becomes blocked/grey while choosing a colour.
- Palette application now closes the colour picker first, restores the parent window, and applies the colour on the next UI cycle.
- Full UI/theme regression passes across Home, Checklists, Projects, Active, Settings, rapid palette switching, direct palette editing, colour-picker focus state, and the overlay.
- Database schema remains unchanged from v0.5.2.

## v0.5.2 — Circular progress + precise scaling + live drag snapping

- Added an exact **Overlay Scale percentage** field in Settings. Any whole percentage from 75% to 150% can be entered directly and persists between launches.
- Overlay scaling is no longer rounded to five-percent steps; it now stores one-percent precision while retaining the slider and quick presets.
- Replaced visible linear completion bars with circular progress rings that display the percentage inside the ring.
- Circular progress is now used on Home project cards, the Projects list, Project Overall Progress, and the floating overlay.
- Removed the visible `Ctrl + Alt + L` shortcut copy from the navigation area, Home overlay button, and tray menu while keeping the shortcut functional.
- Reworked drag/reorder interaction to respond after only a small pointer movement.
- Pressing a draggable block now immediately tints and lifts it with a drop shadow.
- Dragging performs a live snap preview: cards physically move into the proposed insertion position before release.
- The neighbouring drop edge receives an accent outline so the intended position is clear.
- Releasing persists the exact previewed order for Home projects, Project-list cards, and Checklist-template cards.
- Audited palette-token usage so Navigation, Navigation Active, Accent, Panels, Primary Text, and Muted Text map to their labelled palette roles.
- Navigation and Accent foreground/checkmark colours are now derived for contrast instead of borrowing the Panels swatch.
- Updated Settings copy to explain that Navigation/Accent contrast text is derived automatically.
- v0.5.2 UI regression checks pass for circular progress rendering, exact 113% overlay persistence, semantic palette contrast, live Home drag snapping, and all main project/checklist workflows.

## v0.5.1 — Overlay scaling + draggable blocks

- Reworked overlay layout so long project names, checklist names, and checklist items wrap without clipping.
- Overlay section heights now account for wrapped text before cards are drawn.
- Added persistent overlay scaling from 75% to 150%.
- Added a rounded custom Overlay Size slider and 75% / 100% / 125% / 150% quick presets in Settings.
- Expanded and collapsed overlay layouts scale together.
- Added Windows transparent-colour-key rendering so rounded main-window, overlay, and app-dialog corners are genuinely transparent.
- Clicking the main Tickle my List logo now minimises the main window.
- Replaced immediate Exit actions with a consistent rounded confirmation dialog.
- Sidebar Exit, overlay Exit, and tray Exit all use the new confirmation flow.
- Added persistent drag/reorder support for Home project blocks, Project-list cards, and Checklist-template cards.
- Added drag feedback with accent outlines for the source block and current drop target.
- Active Project remains pinned as the featured Home block while the remaining Home blocks can be reordered.
- Added `sort_order` migration columns for projects and checklist templates; the first migration preserves the ordering users saw in v0.5.0.
- Added regression coverage for schema migration, persistent ordering, overlay scale settings, all main screens, overlay construction at 75–150%, and the exit/minimise flows.

## v0.5.0 — Rounded UI + editable base palettes

- Replaced remaining app-owned square-looking buttons with rounded custom buttons.
- Added rounded navigation selection controls.
- Rebuilt checklist-template and project list rows as rounded cards.
- Rebuilt checklist item rows and project checklist rows as rounded cards.
- Added a custom rounded checkbox used in Projects and the floating overlay.
- Replaced native ttk progress bars with rounded custom progress bars.
- Replaced native ttk scrollbars with a slim rounded palette-aware scrollbar.
- Added rounded in-app text prompts for New Project, New Checklist, and Save Palette.
- Rebuilt the palette editor around rounded swatches and visible HEX code fields.
- Removed the native OS colour chooser from palette editing.
- Added an in-app colour dialog with preview, HEX entry, and quick colours from the current palette.
- Light and Dark themes are now persistent editable palettes.
- Added reset-to-default support for Light and Dark.
- Save as Palette now works from Light, Dark, and Custom.
- Added persistent `light_palette` and `dark_palette` settings while keeping the project/checklist schema unchanged.
- Added short fade transitions when applying Light, Dark, Custom, or saved palettes.
- Preserved saved custom palettes and backward compatibility with v0.4.0 settings.
- v0.5.0 regression tests pass for project/checklist data, theme switching, edited Light/Dark persistence, saved palettes, custom controls, and all primary views.


## v0.4.0 — Projects + appearance settings

- Renamed all user-facing Jobs terminology to **Projects** without changing the internal database schema.
- Added a Settings navigation screen.
- Added persistent Light and Dark appearance modes.
- Added Custom palette mode with nine editable core colour tokens.
- Added automatic derivation of supporting theme colours for consistent custom palettes.
- Added Save Palette with persistent named palettes stored in SQLite settings.
- Added a Palettes group containing Light, Dark, and user-saved palettes.
- Removed the Home **Open Job** button.
- Home project cards are now clickable across the entire card surface.
- Added a Settings navigation icon asset.
- Existing checklist/project data remains compatible.
- v0.4.0 UI/theme regression test passes across Home, Checklists, Projects, Active, Settings, Light, Dark, Custom, saved palettes, assignment, and completion tracking.

## v0.3.0 — Approved UI design implementation

- Added the Home dashboard with a grid of real jobs.
- Active Job is highlighted as the featured Home card.
- Rebuilt the main navigation in the approved rounded blue sidebar style.
- Rebuilt Checklist Templates as rounded list/detail panels.
- Rebuilt Jobs as rounded list/detail panels with progress summaries.
- Added a dedicated Active navigation view using the existing Active Job feature.
- Restyled the floating overlay to visually match the new dashboard.
- Retained system tray controls and `Ctrl + Alt + L` overlay shortcut.
- Retained SQLite autosave, daily backups, legacy data migration, and independent job checklist copies.
- Added bundled navigation icon assets.
- Explicitly omitted unsupported mockup fields such as addresses, due dates, comments, notifications, users, search, and extra statuses.
- Database regression test passed.
- Full Home / Checklists / Jobs / Active / Overlay UI construction smoke test passed.

## v0.2.0 — Windows utility pass

- Added Windows system tray controls.
- Added native `Ctrl + Alt + L` shortcut.
- Added overlay position persistence.
- Added branded collapsed overlay.

## v0.1.x

- Initial reusable checklist, jobs, assignment, Active Job, local SQLite storage, backups, and Windows build workflow.
