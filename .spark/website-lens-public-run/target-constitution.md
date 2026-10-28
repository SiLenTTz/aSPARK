# Constitution: startbootstrap-agency (as profiled for this run)

| | |
|---|---|
| **Scope** | The run target only; copied untracked into the scratch clone, never pushed upstream |
| **Owner** | The aSPARK Factory line (dark mode). **Not** the target's maintainers, who never declared this profile |
| **Status** | `active` |
| **Date** | 2026-10-28 |

## 1. Product Principles

- A one-page agency theme must be usable by keyboard and screen-reader users, not only by mouse users.

## 2. Project Profile & Active Lenses

- **Project type(s):** `website`. Evidence: static multi-section page `dist/index.html` built from Pug/SCSS (`package.json`).
- **Characteristics:** `must-be-accessible`. Evidence in the target: none. This is a **declared assumption for the run**: the theme is the starting point of public agency sites, many under an accessibility mandate (EAA/ADA). It is what activates `lenses/accessibility.md` (frontmatter `triggers: [must-be-accessible]`).
- **Active lenses:**

| Lens | Why it's active (or off) | Enforced in |
|---|---|---|
| `accessibility` | **Active** (trigger `must-be-accessible`) | `/story-time`, `/look-and-feel`, `/increment`, `/peer-review`, `/demo-day` |
| `seo`, `ux` | Off on purpose, to keep the run to one lens. Type `website` would otherwise activate them (an override of the default derivation, recorded here) | none |
| `security`, `api`, `cli`, `library`, `i18n`, `data`, `observability` | Off: static page, no auth, no PII, no backend | none |

- **Active-lens load:** 1 lens active.

## 3. Technical Constraints

- Static HTML/CSS/JS in `dist/`; Bootstrap 5.2.3 bundle from a CDN.

## 4. Quality Bars

- WCAG 2.1 AA is the floor (assumed).

## 8. QA Method

- **Browser-observable surface:** yes
