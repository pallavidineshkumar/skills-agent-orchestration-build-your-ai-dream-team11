# Project Pulse implementation plan

## Summary

Build Mona’s Project Pulse as a small, dependency-free static dashboard for contributors. It should present project names, owners, status, recent activity, priority or risk, and a brief contributor-friendly summary in a polished, responsive card layout.

The repository contains the Project Pulse brief, agent definitions, exercise instructions, and validation workflows, but no tracked app source files or `.vscode/launch.json` yet. The Codespace setup script creates `app/` and `docs/`; the dashboard should not require a framework or package installation.

## Ordered steps and file assignments

1. **Agree on the UI and data contract**
   - **Designer:** Define the visual hierarchy, responsive layout, accessible status and priority treatments, and CSS hooks. Coordinate the hooks with Coder—at minimum `.dashboard` and `.project-card`.
   - **Coder:** Confirm the data contract: a top-level `projects` array with `name`, `owner`, `status`, `recentActivity`, and `priority`. Add an optional `summary` field to support the brief’s contributor-friendly summary goal.
   - **Dependency:** Agree on the shared field names and class hooks before integration.

2. **Build the dashboard in parallel**
   - **Designer — `app/styles.css`:** Implement the visual design, including polished project cards, readable spacing and contrast, responsive layout, rounded corners, and shadows. Include `.dashboard` and `.project-card` selectors.
   - **Coder — `app/project-data.json`:** Add several clearly illustrative sample projects with all required fields. Use consistent, understandable status and priority values.
   - **Coder — `app/index.html`:** Create the “Project Pulse” page, link `styles.css`, load `project-data.json`, and render visible `.project-card` elements from its projects. Display the required fields and summary. Use semantic markup and text-safe DOM rendering.
   - **Parallel decision:** CSS and HTML/data can proceed in parallel once the data and class contract is agreed. Keep the HTML/CSS handoff explicit to avoid mismatched selectors.

3. **Configure the runnable preview**
   - **Coder — `.vscode/launch.json`:** Add strict JSON with a **Run Project Pulse Dashboard** configuration. Use `python3 -m http.server 5500`, set `cwd` to `${workspaceFolder}/app`, and configure `serverReadyAction` to open `http://localhost:%s/index.html`.
   - **Dependency:** The launch config can be drafted in parallel, but integration testing depends on the app files and agreed server command being in place.

4. **Integrate and validate**
   - **Coder:** Check that HTML, CSS, data, and launch configuration agree; fix issues within assigned files.
   - **Designer:** Review the rendered hierarchy, responsive behavior, and accessibility.
   - **Orchestrator:** Coordinate the checks below and confirm the launch opens the dashboard rather than a directory listing.

## Parallel versus sequential work

- **Parallel:** Once field names and CSS hooks are agreed, Designer can build `app/styles.css` while Coder builds `app/index.html` and `app/project-data.json`. Coder can also draft `.vscode/launch.json` independently.
- **Sequential:** Agree on the shared UI/data contract before parallel implementation; integrate and run the app only after all files exist. Resolve any mismatch between HTML hooks and CSS before the final preview check.

## Edge cases and risks

- `fetch()` of the JSON data requires serving the page over HTTP; opening `index.html` directly as a `file://` URL may fail. Use the launch configuration.
- Handle failed data loading and an empty `projects` array with a readable message rather than a blank dashboard.
- Keep status and priority distinguishable by both text and visual treatment; do not rely on color alone.
- Treat sample project content as illustrative and replaceable, not authoritative team data.
- `summary` is called for in the brief’s goals but is not in its required field list; treat it as optional so the required five-field schema remains satisfied.
- Port `5500` may already be occupied; stop an existing preview before launching, or resolve the conflict while keeping the readiness pattern and URL consistent.

## Validation expectations

- Confirm all four assigned files exist.
- Run `python3 -m json.tool app/project-data.json` and `python3 -m json.tool .vscode/launch.json`.
- Check that the JSON has a top-level `projects` array and each project has `name`, `owner`, `status`, `recentActivity`, and `priority`.
- Confirm `app/index.html` contains “Project Pulse,” references `styles.css` and `project-data.json`, and renders `.project-card` elements showing status, recent activity, and priority.
- Confirm `app/styles.css` includes `.dashboard`, `.project-card`, `border-radius`, and `box-shadow`.
- Launch **Run Project Pulse Dashboard** in VS Code. Verify the browser opens `/index.html`, displays project cards, and loads the JSON without errors—not the server directory listing. Check a narrow viewport and keyboard-visible focus, then stop the preview server.
- The repository’s Step 3 workflow checks file presence, required phrases, and JSON parsing; it does not run the app. A successful workflow therefore does not replace the manual launch and browser check.

## Open questions

No blocking questions. Confirm whether Mona has real project data and preferred status/priority labels before replacing the illustrative sample data.
