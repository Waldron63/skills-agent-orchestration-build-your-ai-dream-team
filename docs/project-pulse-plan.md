# Project Pulse implementation plan

## Goal

Build Mona's lightweight, static Project Pulse dashboard for contributors. The first view should be a polished, responsive dashboard that makes active projects, owners, status, recent activity, priority/risk, and short summaries easy to scan. The Orchestrator coordinates the work; Planner owns this plan; Designer defines the experience; Coder implements and validates the assigned files.

## Phases and file assignments

1. **Plan and align — Planner**
   - Confirm the brief, repository conventions, data shape, accessibility needs, launch behavior, and validation criteria.
   - No implementation files are modified.

2. **Define the experience — Designer**
   - Own the visual and interaction direction for `app/index.html` and `app/styles.css`.
   - Specify information hierarchy, accessible semantics and contrast, responsive layout, readable spacing, project cards, status badges, priority/risk treatment, and contributor-friendly summaries.
   - Ensure deterministic hooks are part of the design: `.dashboard` for the dashboard container and `.project-card` for every project card.
   - The Designer may implement the assigned styling/markup only when the Orchestrator delegates those files; otherwise the Designer supplies decisions to Coder without changing Coder-owned files.

3. **Implement the static app — Coder**
   - Own `app/index.html`: create the exact `Project Pulse` title, accessible dashboard structure, stylesheet reference, JSON data loading/reference, and visible cards that show each project's name, owner, status, `recentActivity`, priority, and summary.
   - Own `app/styles.css`: implement the Designer's polished responsive layout, including `.dashboard`, `.project-card`, status badge states, priority treatment, readable typography/spacing, rounded corners, shadows, contrast, and usable narrow-screen behavior.
   - Own `app/project-data.json`: create valid JSON with a top-level `projects` array; each project includes `name`, `owner`, `status`, `recentActivity`, and `priority`, with deterministic contributor-oriented sample content.
   - Own `.vscode/launch.json`: create strict JSON with the `Run Project Pulse Dashboard` configuration, serve from `${workspaceFolder}/app`, run `python3 -m http.server 5500`, and open `http://localhost:%s/index.html` so the dashboard opens instead of a directory listing.

4. **Integrate and verify — Orchestrator with Coder**
   - Review all assigned files together, resolve any markup/data/style mismatches, and confirm the final dashboard remains within the stated file scope.

## Responsibilities

**Designer:** establish information hierarchy and visual language; make status and priority legible without relying on color alone; define accessible labels, focus/reading order, responsive breakpoints, card layout, spacing, contrast, and interaction expectations. Keep the first viewport recognizably a Project Pulse dashboard rather than a bare page.

**Coder:** implement the static frontend and launch support exactly within the assigned files; keep data and rendering deterministic; use semantic, accessible markup; connect `project-data.json` to the UI; preserve the required hooks and visible fields; use strict launch JSON; and report validation results and any remaining risk.

## Dependencies and work ordering

- `app/project-data.json` is the content contract for `app/index.html`; agree on its schema and representative records before finalizing card rendering.
- Designer decisions should precede final styling and markup implementation. Designer and Coder may work in parallel only while the Designer produces guidance and Coder prepares the data contract or non-conflicting launch configuration.
- Final `app/index.html` and `app/styles.css` work is sequential after the design handoff because both files share the visual and structural contract.
- `app/project-data.json` and `.vscode/launch.json` can be created in parallel with design work because they have separate file scopes, but launch verification depends on the completed `app/` directory.
- Integration and runtime checks are sequential after all four assigned files exist. No agent should modify another agent's assigned file without an explicit Orchestrator handoff.

## Validation expectations

- Confirm `app/index.html` exists, uses the exact `Project Pulse` title, references `styles.css` and `project-data.json`, contains `.project-card` markup, and visibly renders status, recent activity, and priority.
- Confirm `app/styles.css` contains `.dashboard` and `.project-card`, plus polished rounded/shadowed card styling, status/priority treatments, accessible contrast, and responsive layout rules.
- Parse `app/project-data.json` as JSON; confirm the top-level `projects` array and required fields on every project.
- Parse `.vscode/launch.json` as strict JSON; confirm the exact launch name, `cwd` of `${workspaceFolder}/app`, `python3 -m http.server 5500`, and a server-ready URL ending in `/index.html`.
- Run the repository's exercise validation script where available, then launch **Run Project Pulse Dashboard** and verify the browser shows the dashboard frontend rather than a directory listing. Check representative wide and narrow viewport layouts and keyboard/readability basics.
- Report failures explicitly; do not claim runtime validation if the launch environment is unavailable.
