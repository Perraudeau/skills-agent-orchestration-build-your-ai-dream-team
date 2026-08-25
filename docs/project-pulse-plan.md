# Project Pulse Dashboard Implementation Plan

## Summary

Build Mona’s lightweight, static Project Pulse dashboard for contributors. The dashboard must show active projects, owners, statuses, recent activity, priority/risk, and contributor-friendly summaries using a polished, accessible card-based layout.

Required files:

- `app/index.html`
- `app/styles.css`
- `app/project-data.json`
- `.vscode/launch.json`

The app must run through the **Run Project Pulse Dashboard** VS Code configuration, serve from `app/`, and open `index.html` rather than a directory listing.

## Agent Responsibilities

### Planner

- Define implementation phases and file ownership.
- Identify dependencies, sequencing, parallel work, edge cases, and validation criteria.
- Ensure all required files and dashboard requirements are covered.

### Designer

- Define the information hierarchy and contributor-friendly interaction flow.
- Guide accessible semantic markup, readable typography, color contrast, status badges, priority treatment, spacing, and responsive behavior.
- Specify the visual direction for `.dashboard` and `.project-card`.
- Review the integrated UI for usability and polish.
- Work only within assigned design-related files unless explicitly reassigned.

### Coder

- Implement the static dashboard within the assigned files.
- Connect `index.html` to `styles.css` and `project-data.json`.
- Render visible project cards containing project name, owner, status, recent activity, and priority.
- Create deterministic, strict JSON for `.vscode/launch.json`.
- Configure the launch task to run `python3 -m http.server 5500` from `${workspaceFolder}/app` and open `http://localhost:%s/index.html`.
- Validate syntax, data shape, file references, and launch configuration behavior.
- Report completed validation and remaining risks.

## Ordered Implementation Steps

### 1. Confirm requirements and establish ownership

**Files:** `docs/project-pulse-plan.md` (this plan)

Document the brief, required fields, visual expectations, launch behavior, agent roles, and validation gates.

**Dependencies:** None.

### 2. Define the data contract

**Assigned file:** `app/project-data.json`  
**Owner:** Coder, with Designer review of labels and contributor readability.

Create a top-level `projects` array. Each project object must contain:

- `name`
- `owner`
- `status`
- `recentActivity`
- `priority`

Use multiple representative projects and consistent status/priority values. Keep the data static, valid JSON, and suitable for rendering without additional services.

**Dependencies:** Step 1.

### 3. Define the UI structure and design direction

**Assigned file:** `app/index.html`  
**Design owner:** Designer  
**Implementation owner:** Coder

Specify a semantic dashboard structure with:

- A clear page title: `Project Pulse`
- Introductory contributor-facing context
- A project collection area using the `.dashboard` hook
- One `.project-card` per project
- Visible name, owner, status, recent activity, priority/risk, and summary information
- Accessible headings, labels, landmarks, and meaningful text alternatives where needed

The page must reference both `styles.css` and `project-data.json`, and must render visible project cards from the project data.

**Dependencies:** Step 2 supplies the data contract. The exact data-loading/rendering approach must be agreed before implementation.

### 4. Create the visual system

**Assigned file:** `app/styles.css`  
**Owner:** Designer  
**Implementation support:** Coder only if explicitly delegated.

Provide polished dashboard styling, including:

- `.dashboard` layout hook
- `.project-card` styling hook
- Responsive grid or flex behavior
- Clear status and priority treatments
- Readable spacing and typography
- Rounded corners via `border-radius`
- Depth via `box-shadow`
- Sufficient color contrast
- Keyboard/focus visibility
- Small-screen support without horizontal scrolling
- Reduced-motion consideration for any transitions

**Dependencies:** Step 3 establishes the markup hooks and content structure. Styling can be drafted in parallel with Step 3 only if the agreed class names and semantic structure are stable; final integration is sequential.

### 5. Configure the runnable preview

**Assigned file:** `.vscode/launch.json`  
**Owner:** Coder

Create strict JSON with no comments. Add a configuration named exactly **Run Project Pulse Dashboard**. It must:

- Serve from `${workspaceFolder}/app`
- Run `python3 -m http.server 5500`
- Open `http://localhost:%s/index.html`
- Ensure the browser opens the dashboard frontend, not the app directory listing

**Dependencies:** Independent of visual styling, but must be completed before integrated runtime validation.

### 6. Integrate and review

**Files:** All four assigned files  
**Owners:** Coder integrates; Designer reviews presentation and accessibility; Orchestrator resolves conflicts.

Verify that:

- Data keys match the fields consumed by the page.
- Every project renders as a visible card.
- CSS hooks match the markup.
- No broken relative paths exist.
- The launch working directory and URL match the static app layout.
- The first viewport clearly communicates that this is Project Pulse.

**Dependencies:** Steps 2–5 must be complete. This step is sequential.

### 7. Validate and hand off

**Files:** No implementation changes unless defects are found; validation covers all four assigned files.

Run syntax and structural checks, launch the preview, inspect the rendered dashboard, and record results for the Orchestrator. Stop the preview server after testing.

**Dependencies:** Step 6.

## Dependencies

- `app/project-data.json` defines the fields consumed by `app/index.html`.
- `app/index.html` defines the hooks and structure consumed by `app/styles.css`.
- `.vscode/launch.json` depends on the `app/` directory structure and must target `index.html`.
- Integrated validation depends on all four files being present and internally consistent.
- Designer guidance should precede final Coder implementation, but CSS drafting and data authoring may proceed concurrently after requirements are agreed.

## Parallel Work

The following can run in parallel after Step 1:

- Designer drafts information architecture and visual/accessibility guidance for `app/index.html` and `app/styles.css`.
- Coder authors `app/project-data.json`.
- Coder drafts `.vscode/launch.json`, since it has no dependency on styling.

The Designer and Coder must not simultaneously edit the same file without explicit coordination.

## Sequential Work

1. Requirements and ownership precede implementation.
2. Data contract must be established before finalizing rendering logic.
3. Markup hooks must be agreed before final CSS integration.
4. All implementation must be integrated before runtime and accessibility validation.
5. Defects found during validation must be fixed before handoff.

## Edge Cases and Risks

- Missing, empty, or malformed `projects` data.
- Projects missing one or more required fields.
- Long project names, owner names, or activity text causing overflow.
- Unknown status or priority values needing a neutral fallback style.
- Empty project lists requiring a useful empty state.
- Broken relative references when opened from the wrong directory.
- Directory listing displayed if the launch URL omits `index.html`.
- Port `5500` already being in use.
- Insufficient color contrast or status conveyed by color alone.
- Keyboard focus and narrow viewport layout regressions.
- Strict JSON failure caused by comments, trailing commas, or invalid escaping.
- CORS or local-file restrictions if the page loads JSON through `fetch`; validation should use the configured HTTP server.

## Validation Expectations

### Static validation

- Confirm all required files exist.
- Parse `app/project-data.json` with a JSON parser.
- Parse `.vscode/launch.json` with a JSON parser.
- Confirm `projects` is a top-level array.
- Confirm every project contains `name`, `owner`, `status`, `recentActivity`, and `priority`.
- Confirm `index.html` contains the exact title `Project Pulse`.
- Confirm `index.html` references `styles.css` and `project-data.json`.
- Confirm project cards use `project-card`.
- Confirm CSS contains `.dashboard`, `.project-card`, `border-radius`, and `box-shadow`.
- Confirm launch configuration name, command, working directory, and `index.html` URL.

### Runtime and UX validation

- Start **Run Project Pulse Dashboard**.
- Confirm the browser opens `http://localhost:5500/index.html`.
- Confirm the page shows the dashboard rather than a directory listing.
- Confirm multiple project cards are visible.
- Confirm each card displays status, recent activity, priority, owner, and name.
- Test desktop and narrow viewport layouts.
- Test keyboard navigation and visible focus.
- Check contrast, heading order, readable spacing, and overflow behavior.
- Check the empty-data and unusual-text states if implemented.
- Stop the server after validation.

## Open Questions

- Should project summaries be a separate required data field, or should they be derived from `recentActivity`?
- Which status and priority vocabulary should Mona’s team standardize on?
- Should the dashboard include filtering or sorting, or remain a read-only first version?
- Is a fixed port of `5500` guaranteed to be available in the learner environment?
- Which browser should be the primary target for launch validation?
