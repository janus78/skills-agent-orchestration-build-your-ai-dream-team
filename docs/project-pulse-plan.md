# Mona's Project Pulse Dashboard — Implementation Plan

## Summary
Build a small static Project Pulse dashboard that helps contributors see active projects, owners, status, recent activity, priority or risk, and a brief summary. Follow the repository brief and Step 3 checks: use HTML, CSS, and JSON; render visible project cards; and provide a VS Code launch configuration named **Run Project Pulse Dashboard** that serves from `app/` and opens `index.html`.

No application framework or package setup is present or required. The exercise instructions specify Python's built-in HTTP server for local preview. The existing workflow checks JSON syntax and key dashboard markup and styling requirements; those checks complement, but do not replace, a browser preview.

## Ordered implementation steps

### 1. Agree on the integration contract
**Owner:** Orchestrator, informed by Planner, Designer, and Coder
**Files:** None of the four implementation files

Before specialists edit files, confirm a compact contract to prevent mismatches:
- The data file has a top-level `projects` array.
- Every project has `name`, `owner`, `status`, `recentActivity`, and `priority`.
- Include a short contributor-friendly `summary` as well: it is called for by the brief but is not in the brief's minimum field list.
- Agree on CSS hooks for the dashboard and repeated cards, including `.dashboard` and `.project-card`, and on hooks for status and priority treatments.
- Agree that the page loads JSON at runtime and displays explicit loading, empty, and error states.

**Acceptance:** Designer and Coder can work to the same data shape and CSS hooks without taking ownership of the same file.

### 2. Produce the styles and independent support files
**Owners and file assignments:**
- **Designer:** `app/styles.css` only.
- **Coder:** `app/project-data.json` and `.vscode/launch.json` only.

These tasks can run in parallel after Step 1's contract is agreed.

**Designer — `app/styles.css`:**
- Create a polished, responsive dashboard presentation, not a bare page.
- Style `.dashboard` and `.project-card`; include rounded card corners and shadows.
- Establish clear type hierarchy and spacing for heading, project name, owner, summary, activity, status, and priority.
- Make status and risk/priority treatments distinguishable without relying on color alone.
- Include visible focus states, readable contrast, and layouts for narrow and wide viewports.
- Avoid adding scripts, data, or HTML to the stylesheet.

**Coder — `app/project-data.json`:**
- Provide a top-level `projects` array with several useful sample records for Mona's team.
- Include the five required fields plus `summary` in each record.
- Use concise, contributor-friendly values and consistent status and priority vocabulary.
- Keep the file strict JSON.

**Coder — `.vscode/launch.json`:**
- Add a strict JSON launch configuration named exactly **Run Project Pulse Dashboard**.
- Follow the exercise's specified command, `python3 -m http.server 5500`.
- Set `cwd` to `${workspaceFolder}/app`.
- Configure the server-ready action to open `http://localhost:%s/index.html`, so the preview opens the dashboard rather than a directory listing.
- Use a readiness pattern that captures the server port, and verify the launch configuration works in the repository's VS Code/Codespaces environment.

**Acceptance:** The data parses as JSON and satisfies the agreed schema. The launch file parses as JSON, serves from `app/`, and opens `index.html` at the expected local URL. The stylesheet provides the contracted hooks and responsive dashboard styling.

### 3. Implement page structure and behavior
**Owner:** Coder
**File:** `app/index.html` only
**Dependency:** Step 1's contract and Step 2's agreed hooks and data schema

- Give the page the exact visible title **Project Pulse** and a clear dashboard heading.
- Reference `styles.css` and load `project-data.json`.
- Render project cards from `projects`; show name, owner, summary, status, recent activity, and priority.
- Use the agreed CSS hooks, including `.project-card`, so the Designer's styling applies without stylesheet edits by Coder.
- Use semantic page structure and accessible text labels.
- Provide an understandable loading state while data is read and a clear error state if fetch or JSON parsing fails.
- Handle an empty projects array with a useful empty state rather than a blank dashboard.
- Treat unexpected or missing record values safely; do not inject untrusted strings as HTML.

**Acceptance:** The page loads local JSON when served through the launch configuration and renders a card per project with required visible fields. Loading, empty, and fetch/parse error conditions do not leave a blank or misleading interface.

### 4. Integrate and validate
**Owner:** Orchestrator coordinates; Coder and Designer resolve issues only within their assigned files
**Files:** All four implementation files, with ownership unchanged
**Dependency:** Steps 2 and 3 complete

- Run the repository's applicable Step 3 checks or equivalent checks:
  - `python3 -m json.tool app/project-data.json`
  - `python3 -m json.tool .vscode/launch.json`
  - Confirm `index.html` references `styles.css` and `project-data.json`.
  - Confirm `.dashboard`, `.project-card`, `border-radius`, and `box-shadow` appear in the stylesheet.
  - Confirm project data contains the required top-level array and fields.
- Start **Run Project Pulse Dashboard** in VS Code and confirm the browser opens `index.html` from the `app/` server root.
- Review the rendered page at desktop and narrow viewport sizes, including card readability, contrast, status/priority differentiation, and keyboard focus.
- Exercise empty-data and fetch/parse-error behavior if practical. Record any environment-dependent limitation rather than silently treating it as validated.

**Acceptance:** Static checks pass, the dashboard is visually coherent at representative viewport sizes, and the configured launch action opens the frontend successfully.

## File assignments and non-overlapping ownership

| File | Owner | Responsibility |
|---|---|---|
| `app/index.html` | Coder | Semantic page structure, data loading, card rendering, visible fields, and loading/empty/error behavior. |
| `app/styles.css` | Designer | Dashboard styling, visual hierarchy, responsive layout, accessibility-related visual treatment, and agreed CSS hooks. |
| `app/project-data.json` | Coder | Strict JSON sample project data following the agreed schema. |
| `.vscode/launch.json` | Coder | Strict JSON launch configuration, local server settings, working directory, and dashboard URL opening. |

Designer recommendations should inform the agreed contract and styling; Designer does not edit `index.html`. Coder does not edit `styles.css`. Orchestrator coordinates integration and does not take over implementation files.

## Dependencies and parallel work
- **Sequential:** Step 1's shared contract must precede implementation so data fields and CSS hooks agree.
- **Parallel after Step 1:** Designer can create `app/styles.css` while Coder creates `app/project-data.json` and `.vscode/launch.json`; these scopes are independent.
- **Sequential after Step 2:** Coder completes `app/index.html` against the agreed data schema and Designer's delivered hooks. No overlapping edits.
- **Sequential final gate:** Integrated validation follows completion of all four files. Fixes remain within each file's assigned owner scope.

## Edge cases and risks
- Opening with `file://` may prevent runtime JSON fetching; use the configured local HTTP server.
- Missing or malformed JSON, failed fetch, or invalid data must produce a visible error state.
- An empty `projects` array should produce a useful empty state.
- Missing or unexpected values should not break rendering; avoid unsafe HTML insertion.
- Status and priority vocabularies are unspecified. Keep sample values consistent and ensure unknown values remain understandable.
- The brief requests a contributor-friendly summary but does not name its JSON property; this plan proposes `summary` in addition to the five required fields.
- Verify the selected VS Code launch configuration type and server-ready pattern work in the target Codespace; valid JSON alone does not prove launch/browser behavior.
- Exercise workflows check specified strings and JSON parsing but do not run the app or prove runtime behavior.

## Validation expectations
1. **Static checks:** Both JSON files parse; required fields, selectors, title, links, and launch name are present, consistent with `.github/workflows/3-step.yml`.
2. **Launch check:** **Run Project Pulse Dashboard** starts the server with `app/` as working directory and opens `index.html`, not a directory listing.
3. **Browser check:** Confirm cards show name, owner, summary, status, recent activity, and priority.
4. **UX/accessibility check:** Review narrow and wide layouts, text readability, keyboard focus, and status/priority meaning without color alone.
5. **Error-path check:** Confirm empty data and load/parse failures are communicated visibly and do not cause a misleading blank dashboard.

## Open questions
- Which status and priority values should sample projects use? The brief does not prescribe allowed values.
- How many sample projects best demonstrate the dashboard without implying real project information?
- Should `summary` be required for each record as proposed, or optional?
- Are `python3` and the expected VS Code launch support available in every intended learner environment? Verify in the target Codespace during integration.
