## Repo snapshot

This is a small static frontend project (no JS toolchain detected). Key files:

- `index.html` — page markup.
- `style.scss` — main SCSS source (edit this file; it compiles to `style.css`).
- `style.css` — generated CSS (do not edit directly unless you intentionally stop using SCSS).
- `read.md` — project notes the author left; check it for design intent.

## Big picture / architecture

- Single-page static site with styles written in SCSS. No build scripts or package.json detected in the repository root.
- Styling is written using nested SCSS with ID and class-heavy selectors (e.g. `#sec4 .boxes #img .hoverclass`).
- There are no backend services, API calls, or tests in the codebase—changes are verified by opening `index.html` in a browser.

## Developer workflows (explicit, reproducible)

- Compile SCSS to CSS (recommended): use the Dart Sass CLI. From the project folder run (PowerShell):

  sass style.scss style.css --no-source-map

  If you prefer watching for changes:

  sass --watch style.scss:style.css

- Manual verification: open `index.html` in a browser and use DevTools (Elements / Styles) to inspect CSS changes.

## Project-specific conventions and patterns

- Preference for nested SCSS and ID-based structure. Example pattern in `style.scss`:

  #sec4 {
    .boxes {
      #img { ...
        img { ... }
        .hoverclass { ... }
      }
    }
  }

- The code uses Google Fonts imported at the top of `style.scss`.
- Many interactive effects are implemented in CSS via hover and transform — be mindful of selector scoping when changing nesting.

## Common pitfalls observed (be explicit)

- Hover reveal bug pattern: `img { &:hover .hoverclass { ... } }` appears in `style.scss` (lines ~294–357). That selector only matches a `.hoverclass` inside the `img` element — but `.hoverclass` is a sibling/child of the parent container, not a descendant of the `img`. To make the overlay appear when the parent is hovered, prefer one of these selectors:

  /* target overlay when parent #img is hovered */
  #img:hover .hoverclass {
    transform: translateY(0);
    display: block;
  }

  Or adjust nesting in SCSS with a parent reference:

  #img {
    &:hover { .hoverclass { transform: translateY(0); display: block; } }
  }

  Fixing this will make the gradient overlay reliably appear on hover.

## Where to edit for common tasks

- Change layout or content: edit `index.html`.
- Change styling: edit `style.scss` and recompile to `style.css` using the Sass CLI.
- Quick checks: open `index.html` locally. No dev server is required for basic work.

## Integration and external dependencies

- External fonts loaded from Google Fonts via `@import` in `style.scss`.
- No build tools or package managers detected — if adding dependencies, add `package.json` and document commands here.

## Minimal examples for the agent

- If the task is "fix hover overlay on cards", open `style.scss` and replace the nested `&:hover .hoverclass` rule (around lines 294–357) with the `#img:hover .hoverclass` pattern above.
- If asked to add a new card, follow existing HTML structure in `index.html` and the `.boxes` structure in `style.scss` so styles apply by convention.

## When to ask the user

- If changes require a build tool (e.g., switch to a Node-based toolchain), ask whether you should add `package.json` and which tool (webpack, parcel, vite, or plain sass) they prefer.
- If design intent in `read.md` is ambiguous (colors, spacing), ask for clarification or a screenshot to match.

---

If you want I can also: add a simple `sass`-watch npm script and a small `package.json`, or apply the hover fix in `style.scss` directly — tell me which you'd prefer.
