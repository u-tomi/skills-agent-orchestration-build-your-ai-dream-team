# Project Pulse implementation plan

## Summary

Mona's team needs a lightweight, polished static dashboard that lets contributors quickly understand active projects, owners, current status, recent activity, priority or risk, and a short contributor-friendly summary. The implementation will use `app/index.html`, `app/styles.css`, and `app/project-data.json`, plus a strict JSON VS Code launch configuration at `.vscode/launch.json`.

The workflow follows the repository's custom-agent model: the **Planner** establishes the implementation strategy, the **Designer** owns the user experience and visual/accessibility decisions, the **Coder** implements the assigned static files and runnable preview support, and the **Orchestrator** coordinates phases, prevents file-scope conflicts, and validates the integrated result. No agent should stage, commit, or push changes.

## Ordered implementation steps

1. **Confirm requirements and repository constraints — Planner**
   - Read `.github/project-pulse-brief.md`, the existing agent definitions, relevant exercise instructions, and available validation scripts.
   - Record the required data contract: a top-level `projects` array with `name`, `owner`, `status`, `recentActivity`, and `priority` for every project.
   - Confirm the launch behavior: serve from `app/`, use `python3 -m http.server 5500`, and open `index.html` through the named **Run Project Pulse Dashboard** configuration.

2. **Define the dashboard experience — Designer**
   - Establish the first-view information hierarchy: clear `Project Pulse` title, concise overview, and immediately visible project cards.
   - Define accessible markup expectations, readable typography and spacing, responsive behavior, status badges, priority treatment, and contributor-friendly summaries.
   - Specify stable styling hooks, including `.dashboard` and `.project-card`, so implementation and validation are deterministic.

3. **Create the project data contract and representative content — Coder**
   - **File assignment: `app/project-data.json` only.**
   - Create valid JSON with multiple representative projects and the required fields.
   - Use consistent, human-readable values for status, recent activity, and priority so every required field can be rendered and visually distinguished.

4. **Implement the visual system — Designer**
   - **File assignment: `app/styles.css` only.**
   - Implement the polished dashboard styling: `.dashboard`, `.project-card`, responsive grid or stack behavior, rounded corners, shadows, contrast, focus states, status badges, and priority treatment.
   - Keep the stylesheet independent of JavaScript and compatible with a small static app.

5. **Implement the dashboard page — Coder**
   - **File assignment: `app/index.html` only.**
   - Use the exact title `Project Pulse`.
   - Reference `styles.css` and `project-data.json`.
   - Render visible cards from the `projects` data rather than leaving project information hidden or represented only by placeholders.
   - Show each project's name, owner, status, recent activity, and priority, using the agreed `.dashboard` and `.project-card` hooks and accessible labels/structure.

6. **Add runnable preview support — Coder**
   - **File assignment: `.vscode/launch.json` only.**
   - Create strict JSON with a configuration named **Run Project Pulse Dashboard**.
   - Set `cwd` to `${workspaceFolder}/app`.
   - Serve with `python3 -m http.server 5500`.
   - Configure `serverReadyAction` to open `http://localhost:%s/index.html`, ensuring the browser opens the dashboard instead of a directory listing.

7. **Integrate and validate — Orchestrator**
   - Review all four assigned implementation files together for contract mismatches.
   - Confirm the data fields map to visible UI, stylesheet hooks match the markup, and the launch configuration targets the correct directory and page.
   - Surface any blocker to the responsible specialist instead of silently changing another agent's file scope.

## Explicit file assignments

| File | Primary owner | Scope and expectations |
| --- | --- | --- |
| `app/index.html` | Coder | Static page shell, exact `Project Pulse` title, stylesheet/data references, accessible project-card markup, and visible rendering of all required project fields. Designer supplies the information hierarchy and markup requirements but does not edit this file unless the Orchestrator explicitly reassigns it. |
| `app/styles.css` | Designer | Complete visual system, responsive layout, readable spacing, `.dashboard`, `.project-card`, rounded corners, shadows, contrast, badges, priority treatment, and focus states. |
| `app/project-data.json` | Coder | Valid top-level `projects` array; every project includes `name`, `owner`, `status`, `recentActivity`, and `priority`. |
| `.vscode/launch.json` | Coder | Strict JSON, **Run Project Pulse Dashboard**, `cwd` set to `${workspaceFolder}/app`, `python3 -m http.server 5500`, and URL ending in `/index.html`. |

## Designer responsibilities

- Make the first view clearly read as a Project Pulse dashboard, not a bare HTML page.
- Define the information hierarchy and interaction/readability expectations for contributors.
- Use visible project cards, status badges, clear priority treatment, readable spacing, rounded corners, shadows, and responsive layout.
- Include accessible structure, adequate contrast, keyboard-visible focus states, meaningful labels, and mobile-friendly behavior.
- Own `app/styles.css` and report design decisions, files touched, tradeoffs, and validation recommendations.

## Coder responsibilities

- Implement only the files assigned by the Orchestrator, following existing repository patterns and the Designer's agreed UI contract.
- Keep the static implementation deterministic, explicit, and easy to validate.
- Create `app/index.html`, `app/project-data.json`, and `.vscode/launch.json` within the assignments above; do not change design-only files unless explicitly assigned.
- Ensure the launch configuration opens the dashboard page, not a server directory listing.
- Validate JSON, references, visible required fields, and launch settings before reporting completion.

## Dependencies and phase gates

- Phase 1 must be sequential and complete before design or implementation begins because it establishes requirements and file ownership.
- The Designer's layout and hook decisions must be available before the Coder finalizes `app/index.html`; this prevents markup/style contract drift.
- `app/project-data.json` must define its field names before `app/index.html` is finalized, because the page must render those exact values.
- `.vscode/launch.json` depends on the known app location and `index.html` entry point, but it does not depend on runtime server behavior during authoring.
- Integration validation must be last and must inspect the combined result rather than validating each file in isolation only.

## Parallel-work decisions

**Can run in parallel**

- After the Planner completes the requirements phase, the Designer can prepare the visual direction and the Coder can create `app/project-data.json` in parallel because they have disjoint files.
- Once the data contract and visual hook names are agreed, the Coder can prepare the launch configuration while the Designer completes `app/styles.css`; these files do not overlap.
- Independent static checks for JSON syntax, CSS selectors, and required text can run in parallel during validation.

**Must run sequentially**

- Planning precedes delegation and implementation.
- Designer hook and accessibility decisions precede final `app/index.html` markup.
- Data-contract agreement precedes page rendering work.
- The Orchestrator's integrated review follows all file changes; launch behavior and cross-file references cannot be considered complete from isolated checks.
- Any correction to a shared contract must be made by the owning agent before final validation, rather than patched concurrently by another agent.

## Validation expectations

- Confirm `app/index.html`, `app/styles.css`, `app/project-data.json`, and `.vscode/launch.json` exist.
- Parse `app/project-data.json` and `.vscode/launch.json` as strict JSON; `launch.json` must contain no comments.
- Confirm the data has a top-level `projects` key and that every project has `name`, `owner`, `status`, `recentActivity`, and `priority`.
- Confirm `index.html` contains the exact `Project Pulse` title, references `styles.css` and `project-data.json`, uses `project-card`, and visibly renders status, recent activity, and priority.
- Confirm `styles.css` contains `.dashboard` and `.project-card`, plus `border-radius`, `box-shadow`, responsive layout rules, readable contrast, and focus treatment.
- Confirm `.vscode/launch.json` names **Run Project Pulse Dashboard**, uses `${workspaceFolder}/app` as `cwd`, runs `python3 -m http.server 5500`, and opens `http://localhost:%s/index.html`.
- Run the repository's existing exercise validation where applicable, then manually start the named launch configuration and verify that the browser shows the Project Pulse UI rather than a directory listing.
- Check representative narrow and wide viewport behavior and keyboard navigation through cards or interactive elements.

## Edge cases and risks

- Missing or misspelled JSON fields can produce incomplete cards; validate every project object against the contract.
- Invalid JSON or comments in `launch.json` can prevent VS Code from loading the configuration; use strict JSON only.
- A launch configuration with the wrong `cwd`, missing `index.html`, or an incorrect `serverReadyAction` URL can expose a directory listing instead of the dashboard.
- Empty project arrays, unusually long project names, long activity text, unknown status values, and high-priority labels must remain readable without breaking the layout.
- Status and priority values may vary in case or wording; styling should remain understandable and should not rely solely on color.
- Narrow screens may cause card overflow if fixed widths are used; use responsive grid/flex rules and allow text to wrap.
- Low contrast, missing focus indicators, or unlabeled status/priority content can make the dashboard inaccessible.
- Overlapping edits to `app/index.html` or `app/styles.css` can erase specialist work; the Orchestrator must enforce the assignments and sequence.
- External assets or network-only data would make the static preview fragile; keep the implementation self-contained unless the Orchestrator explicitly approves a dependency.

## Open questions

- Should the first version include filtering or sorting, or should it remain a read-only overview as specified by the brief? Default: keep the scope to the static overview.
- Should `recentActivity` be plain text or include a date plus summary? Default: use a concise contributor-friendly string that is directly renderable.
- Should unknown status or priority values receive a neutral fallback style? Recommended: yes, so new data does not disappear or become unreadable.
- Is JavaScript rendering expected, or is server-rendered/static markup acceptable? The plan assumes a small static app may use a minimal browser-side data load, but the final implementation must visibly render the JSON-backed project cards.
