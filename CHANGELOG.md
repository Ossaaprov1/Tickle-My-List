## v0.6.3 — Scoped refresh + proportional overlay scaling

- Replaced routine full-page/full-shell refreshes with semantic scoped refresh requests.
- Overlay checkbox changes now refresh the overlay immediately and only refresh the visible full-app page when it shows the affected project.
- Hidden pages are marked dirty and rebuild when navigated to instead of repainting immediately.
- Preserved persistent page surfaces and avoided destroying unrelated widgets for normal project/checklist edits.
- Removed the overlay Pin mode and Pin control; legacy `overlay_pinned` settings are ignored safely.
- Lock is now the single overlay immobility/topmost control: unlocked is movable/resizable and not forced topmost; locked is fixed and always on top.
- Locked overlay hides Snap, Minimise, Open Full App, and Exit, leaving only Unlock.
- Added four-corner proportional resize handling for the expanded overlay while unlocked.
- Corner resizing scales the complete overlay UI together: window, typography, icons, checkboxes, padding, cards, and progress ring.
- Expanded Overlay Scale range to 50%–200% and kept a single persisted `overlay_scale` source of truth.
- Snap remains attached to the current screen edge while resizing and recomputes geometry from the scaled overlay bounds.
- Collapsed overlay remains fixed-size and expanded overlay restores the last saved scale.
- Added regression coverage for lock/topmost semantics, Pin removal, proportional scaling, snap-aware resize geometry, and scoped refresh routing.

## v0.6.2 — Snap, lock cleanup + edge gradient

- Added edge-aware overlay gradient that is strongest at the nearest screen edge and fades inward.
- Added Snap mode and perimeter-constrained dragging around the desktop work area.
- Reworked overlay controls to compact icon buttons with header dragging support.
- Added Pin/Lock interaction rules and locked-state control hiding.

## v0.6.1 — Calendar dates + PDF redesign

- Added calendar selectors and DD-MM-YYYY display formatting.
- Removed the default project status.
- Redesigned ReportLab PDF exports to match the application styling.
- Optional metadata is hidden when unset.
- Main/dialog windows are opaque; only overlay corners remain transparent.
