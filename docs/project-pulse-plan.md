# Project Pulse Dashboard Implementation Plan

## Summary

Build a lightweight, static **Project Pulse** dashboard that helps contributors quickly understand active projects, ownership, current status, recent activity, priority or risk, and a short contributor-friendly summary.

The implementation must create:

- `app/index.html`
- `app/styles.css`
- `app/project-data.json`
- `.vscode/launch.json`

The dashboard should run without a package installation or backend. It should be served from `app/` and open `index.html`, not a directory listing.

## Existing Repository Constraints

- The requirements are defined in `.github/project-pulse-brief.md`.
- Agent roles are defined in `.github/agents/`.
- The Designer is responsible for layout, hierarchy, accessibility, responsiveness, and visual polish.
- The Coder is responsible for implementation and may create `.vscode/launch.json` when assigned.
- The repository has no existing application framework or application implementation to preserve.

## File Assignments

| File | Owner | Responsibility |
| --- | --- | --- |
| `app/index.html` | Coder, guided by Designer | Create the accessible dashboard shell, title, project-card container, rendering logic, stylesheet reference, and data reference. |
| `app/styles.css` | Designer | Define the visual system, responsive layout, typography, status and priority treatments, spacing, focus states, `.dashboard`, `.project-card`, `border-radius`, and `box-shadow`. |
| `app/project-data.json` | Coder, with content guidance from Designer | Provide a valid top-level `projects` array. Each project must include `name`, `owner`, `status`, `recentActivity`, and `priority`; include a contributor-friendly summary if the UI displays one. |
| `.vscode/launch.json` | Coder | Create the `Run Project Pulse Dashboard` configuration, serve from `${workspaceFolder}/app`, and open `index.html` directly. |

The Planner owns this implementation plan. The Orchestrator owns coordination, delegation, integration review, and final handoff documentation.

## Designer Responsibilities

The Designer should:

- Define the information hierarchy for contributors scanning several projects.
- Establish a polished first view with a clear Project Pulse title and supporting context.
- Design a responsive card grid for narrow and wide screens.
- Ensure each project card visibly presents the project name, owner, status, recent activity, priority or risk, and contributor-friendly summary when available.
- Recommend status and priority treatments that do not rely on color alone.
- Specify accessible landmarks, heading order, semantic labels, readable contrast, and visible keyboard focus.
- Ensure the design uses deterministic hooks required by the exercise: `.dashboard` and `.project-card`.
- Review the integrated HTML and CSS for visual consistency without changing files outside the assigned design scope.

## Coder Responsibilities

The Coder should:

- Create the `app/` directory and the three static app files.
- Implement `app/index.html` with the exact `Project Pulse` title, a reference to `styles.css`, a reference to `project-data.json`, and visible project-card rendering.
- Render one `.project-card` per project in the top-level `projects` array.
- Display the project name, owner, status, recent activity, and priority in every card.
- Add explicit loading and error states rather than silently rendering an empty dashboard.
- Create valid JSON data with multiple representative projects and all required fields.
- Create strict JSON in `.vscode/launch.json` with:
  - Configuration name: `Run Project Pulse Dashboard`
  - Working directory: `${workspaceFolder}/app`
  - Command: `python3 -m http.server 5500`
  - A browser action that opens `http://localhost:%s/index.html`
- Keep the app deterministic and dependency-free.
- Validate the integrated output before reporting completion.

## Dependencies

1. `.github/project-pulse-brief.md` and `.github/agents/` establish the required scope and responsibilities.
2. Designer decisions about hierarchy, labels, status treatment, and responsive layout inform `app/index.html` and `app/styles.css`.
3. `app/project-data.json` must use the schema expected by the rendering logic in `app/index.html`.
4. `app/index.html` must reference the exact filenames `styles.css` and `project-data.json`.
5. `.vscode/launch.json` depends on the final location of `index.html` and `project-data.json` under `app/`.
6. Browser testing depends on serving the app over HTTP because direct `file://` access may prevent JSON loading.
7. Final Orchestrator review depends on all implementation files being present and aligned.

## Ordered Implementation Steps

### 1. Orchestrator confirms scope

Review the Project Pulse brief, custom agent definitions, workflow requirements, and validation scripts. Confirm the four required implementation files and give Designer and Coder explicit, non-overlapping scopes.

### 2. Designer defines the dashboard experience

**Owner:** Designer  
**Primary file:** `app/styles.css`  
**Guidance for:** `app/index.html` and `app/project-data.json`

Define the layout, card anatomy, typography, spacing, visual states, responsive breakpoints, and accessibility treatments. Specify the semantic structure and content order expected in each card.

### 3. Coder models representative project data

**Owner:** Coder  
**Primary file:** `app/project-data.json`

Create a top-level `projects` array with multiple projects. Give every project the exact required keys: `name`, `owner`, `status`, `recentActivity`, and `priority`. Include a consistent `summary` field if the final UI requires one.

### 4. Coder implements the HTML and rendering

**Owner:** Coder  
**Primary file:** `app/index.html`  
**Depends on:** Designer’s structure guidance and the agreed data schema

Create accessible page landmarks and a clear Project Pulse heading. Link the stylesheet, load the JSON data, render one `.project-card` per project, display all required fields, and provide loading and error states.

### 5. Designer and Coder integrate the UI

**Owners:** Designer and Coder  
**Files:** `app/index.html`, `app/styles.css`, `app/project-data.json`

Coder applies the design guidance. Designer reviews the HTML/CSS relationship and adjusts design-owned CSS as needed. Confirm `.dashboard`, `.project-card`, `status`, `recentActivity`, `priority`, `border-radius`, and `box-shadow` are present and meaningful.

### 6. Coder creates the launch configuration

**Owner:** Coder  
**File:** `.vscode/launch.json`  
**Depends on:** Final app location and `index.html`

Create strict JSON for `Run Project Pulse Dashboard`, serving from `${workspaceFolder}/app` with `python3 -m http.server 5500` and opening `http://localhost:%s/index.html`.

### 7. Orchestrator performs integration review

Review all four implementation files. Confirm that the launch configuration serves the same directory as the app, the HTML references the exact stylesheet and data filenames, and the JSON schema matches the rendering logic.

### 8. Validate and prepare handoff

Run structural and syntax validation, launch the dashboard through VS Code, confirm that `index.html` opens instead of a directory listing, and record the result for the final handoff.

## Parallel Work Decisions

### Work that can run in parallel

- Designer can define visual direction and accessibility requirements while Coder creates `app/project-data.json`.
- The Orchestrator can review the brief, agent definitions, workflows, and validator while Designer and Coder work.
- Coder can prepare the initial data fixture while Designer prepares CSS and markup guidance because the required data keys are already fixed.

### Work that must be sequential

1. The Orchestrator must establish file ownership before delegated work begins.
2. HTML rendering must follow the agreed data schema and Designer’s content hierarchy.
3. CSS integration must follow the actual markup classes and DOM structure.
4. `.vscode/launch.json` should follow the final app location and target filename.
5. Browser launch validation must occur after all four files exist.
6. Final handoff documentation must follow integration review and validation.

Designer and Coder should not edit the same file concurrently. The Designer should own CSS changes while the Coder owns HTML, JSON, and launch configuration changes unless the Orchestrator explicitly changes the scope.

## Edge Cases and Risks

- `project-data.json` may be missing, malformed, served from the wrong path, or missing the `projects` array.
- Projects may omit required fields or contain unusually long text that overflows cards.
- Unexpected status or priority values should remain readable and should not depend only on color.
- Direct filesystem opening may cause `fetch()` to fail; the supported path is the HTTP server launched through VS Code.
- Data-loading failures must produce a clear user-visible error state.
- `launch.json` must be strict JSON with no comments or trailing syntax.
- The launch configuration must use `${workspaceFolder}/app` and open `/index.html`, not just the directory.
- Port `5500` may already be occupied and should be reported rather than silently changing the required port.
- Do not modify `.vscode/tasks.json` or add unrelated frameworks, dependencies, or generated files.

## Validation Expectations

### Structural validation

Confirm that these files exist:

- `app/index.html`
- `app/styles.css`
- `app/project-data.json`
- `.vscode/launch.json`

Confirm that:

- `app/index.html` contains `Project Pulse`.
- `app/index.html` references `styles.css` and `project-data.json`.
- Project cards use the `project-card` class and display status, recent activity, and priority.
- `app/styles.css` contains `.dashboard`, `.project-card`, `border-radius`, and `box-shadow`.
- `app/project-data.json` contains a top-level `projects` array.
- Every project contains `name`, `owner`, `status`, `recentActivity`, and `priority`.
- `.vscode/launch.json` contains `Run Project Pulse Dashboard` and `index.html`.

### Syntax validation

Use the repository’s existing validation where appropriate:

```bash
python3 -m json.tool app/project-data.json >/dev/null
python3 -m json.tool .vscode/launch.json >/dev/null
bash scripts/validate-exercise.sh
```

If the broad exercise validator reports an unrelated pre-existing or environment-specific issue, record it rather than changing unrelated files.

### Runtime validation

1. Start `Run Project Pulse Dashboard` in VS Code.
2. Confirm the server starts on port `5500`.
3. Confirm the browser opens `http://localhost:5500/index.html`.
4. Confirm the Project Pulse UI appears instead of a directory listing.
5. Confirm multiple project cards are visible.
6. Confirm each card displays owner, status, recent activity, priority, and summary content where provided.
7. Check narrow and wide viewport layouts.
8. Confirm loading and error behavior is explicit if the data file cannot be loaded.
9. Stop the preview server after validation.

## Open Questions

- The brief requires a contributor-friendly summary but does not define a required JSON key. The Orchestrator should decide whether to add a consistent optional `summary` field or derive a summary from required fields.
- The brief does not prescribe exact status or priority vocabularies. The Coder should choose a small consistent vocabulary and the Designer should style it so the meaning remains clear through text.
- The launch configuration requires `python3`; if the environment lacks it, report the limitation instead of silently changing the required command.
