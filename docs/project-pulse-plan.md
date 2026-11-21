# Project Pulse Implementation Plan

## Goal

Create a lightweight, polished static Project Pulse dashboard that gives Mona's contributors a short, readable overview of active projects: who owns each project, its current status and recent activity, and its priority or risk. Cards may include a concise, contributor-friendly `summary` to make each project's purpose easier to grasp at a glance. The finished app must be visible as a card-based dashboard at `app/index.html` and must launch in VS Code through a dedicated Project Pulse preview.

## Implementation phases

### Phase 1: Define the data contract

The Coder creates `app/project-data.json` first because the markup and dashboard rendering depend on its shape.

- Use a top-level `projects` array.
- Give every project the required core fields: `name`, `owner`, `status`, `recentActivity`, and `priority`; do not remove or rename these fields.
- Optionally add a concise `summary` value to a project as an additional contributor-friendly description; omit or safely handle it when unavailable rather than replacing any required field.
- Include multiple representative projects, varied statuses, and varied priorities so the dashboard behavior and visual states can be validated.
- Keep values concise and deterministic; use valid JSON without comments or trailing commas.

### Phase 2: Build the accessible dashboard markup

The Coder creates `app/index.html` after or together with the data contract:

- Use the exact document title **Project Pulse**.
- Link `styles.css`.
- Load or reference `project-data.json` and render visible project cards from that data.
- Give every project card the exact `project-card` class.
- Present `name`, `owner`, `status`, `recentActivity`, and `priority` for every project.
- When a project has a valid optional `summary`, present it clearly in the card; when absent or malformed, omit it or use a safe fallback without displaying `undefined`.
- Use semantic headings and an appropriate main/dashboard landmark, with readable text and accessible status labels.
- Provide explicit empty/error/loading behavior so malformed or unavailable data does not leave the page blank or silently fail.

### Phase 3: Apply the visual system

The Designer owns the visual and accessibility decisions and the Coder implements them in `app/styles.css`.

- Include the exact `.dashboard` selector and `.project-card` selector.
- Add border-radius, box-shadow, clear typography, strong contrast, and readable spacing.
- Show status and priority with clear badges or other strong visual affordances.
- Provide a responsive layout that uses one or more columns appropriately across desktop and mobile widths.
- Preserve keyboard navigation and avoid color-only meaning for status or priority.

### Phase 4: Add the runnable preview

The Coder creates `.vscode/launch.json` as strict JSON with no comments:

- Name the configuration **Run Project Pulse Dashboard**.
- Set `cwd` to `${workspaceFolder}/app`.
- Use the command `python3 -m http.server 5500`.
- Configure `serverReadyAction` to open `http://localhost:%s/index.html`.
- Ensure the launch opens the dashboard frontend rather than the server directory listing.

### Phase 5: Integrate and validate

The Orchestrator verifies that data, markup, styles, and launch behavior are integrated. The Coder performs runnable checks and the Designer reviews visual/accessibility outcomes.

## File assignments

| File | Owner | Responsibility |
|---|---|---|
| `app/project-data.json` | Coder | Define the top-level `projects` array and each project's required `name`, `owner`, `status`, `recentActivity`, and `priority` fields, with an optional concise `summary` value when useful. |
| `app/index.html` | Coder | Build the semantic Project Pulse title and dashboard structure, fetch or load the JSON, and render every project as a `.project-card`. |
| `app/styles.css` | Coder with Designer guidance | Implement the `.dashboard` layout, `.project-card` styling, badges, spacing, contrast, and responsive behavior. |
| `.vscode/launch.json` | Coder | Create strict JSON launch support for the named server preview from `app/` on port 5500. |

## Designer responsibilities

The Designer guides UI/UX, information hierarchy, accessibility, interaction flow, visual clarity, and responsive behavior.

- Decide the card hierarchy and the presentation of status, priority, and optional contributor-friendly summaries.
- Ensure the Project Pulse title and project fields are easy to scan.
- Specify polished CSS affordances: rounded corners, shadows, readable typography, and sufficient contrast.
- Review responsive behavior and state presentation.
- Avoid decorative changes that reduce usability or hide project information.
- Report design decisions and remaining accessibility risks; do not stage or commit files.

## Coder responsibilities

The Coder implements the static app and runnable support within the assigned files.

- Keep markup, data, CSS, and launch configuration consistent and predictable.
- Implement simple, explicit rendering logic and informative failure states.
- Keep JavaScript behavior deterministic and testable, if JavaScript is used.
- Use strict JSON for both project data and `.vscode/launch.json`.
- Validate syntax and run behavior before reporting completion.
- Do not change design-only decisions outside the assigned implementation scope and do not stage, commit, or push.

## Dependencies

- `app/index.html` depends on `app/project-data.json` for project content and on `app/styles.css` for presentation.
- `app/styles.css` depends on the markup and class names established in `app/index.html`, especially `.dashboard` and `.project-card`.
- The launch configuration depends on `app/index.html` existing and Python 3 being available.
- The launch preview depends on port 5500 being available and on serving with `app/` as the working directory.
- The final visual/accessibility review depends on the data and markup having been integrated.

## Parallel work decisions

- Parallel: Designer and Coder may agree on card structure, field presentation, and CSS hooks while the Coder establishes `app/project-data.json`.
- Parallel: after the JSON contract and required HTML structure are agreed, CSS work may proceed alongside final markup refinements.
- Parallel: the Coder may prepare `.vscode/launch.json` once the app file paths and launch command are known.
- Sequential: project data should precede end-to-end rendering validation because the dashboard must render real records.
- Sequential: launch validation must follow creation of `app/index.html` and `app/styles.css`, so the browser can open a complete frontend.
- Sequential: final verification should happen after all four files exist rather than treating each file as an isolated deliverable.

## Edge cases

- A project array may be empty: show a clear no-project message rather than an empty dashboard.
- JSON may fail to load or parse: show a concise, visible error without crashing the page.
- Missing optional or malformed project fields: do not render `undefined` text; show a safe fallback such as “Unknown” or “Not provided.” Omit an optional `summary` or use “Summary not provided” instead of inventing project information.
- Optional summaries: keep project summaries concise and contributor-friendly, and use them only as supplemental context alongside every required field.
- Long project names, owner names, and activity text: wrap or truncate safely while preserving readable access to the content.
- Status or priority text may be unknown: use neutral styling and text labels instead of assuming a fixed known status.
- Small screens: retain card readability and avoid horizontal overflow.
- Port 5500 already in use: report the conflict and stop; do not silently switch ports because 5500 is a repository requirement.
- Browser cache or stale preview: reload the direct index URL and verify the response comes from the app directory.

## Validation expectations

### Structural validation

- Confirm `app/index.html` contains the exact title “Project Pulse”, links `styles.css`, and loads or references `project-data.json`.
- Confirm visible project cards use the `project-card` class and show status, recentActivity, and priority values.
- Confirm `app/styles.css` contains both `.dashboard` and `.project-card`, plus border-radius, box-shadow, and responsive rules.
- Parse `app/project-data.json` and confirm it contains a top-level `projects` array.
- Confirm every project object contains `name`, `owner`, `status`, `recentActivity`, and `priority`.
- Confirm an optional `summary` is concise when present and that cards remain complete and readable if `summary` is omitted or malformed.
- Confirm `.vscode/launch.json` parses as strict JSON with no comments.

### Launch validation

- Confirm the configuration is named **Run Project Pulse Dashboard**.
- Confirm `cwd` is `${workspaceFolder}/app`.
- Confirm the command is exactly `python3 -m http.server 5500`.
- Confirm `serverReadyAction` opens `http://localhost:%s/index.html`.
- Launch from VS Code Run and Debug, stop the server afterward, and verify the browser opens the Project Pulse UI rather than a directory listing.

### Behavior and accessibility validation

- Verify project cards are visible with representative data.
- Verify status and priority remain understandable through text, not color alone.
- Verify keyboard and screen-reader structure is sensible, with semantic headings and no inaccessible controls.
- Check desktop and narrow viewport layouts for overflow, clipping, and unreadable spacing.
- Verify empty, unavailable, and malformed data states remain clear and non-crashing.
