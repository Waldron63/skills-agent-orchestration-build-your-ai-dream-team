# Project Pulse design handoff

This document defines the visual, semantic, and interaction contract for Mona's
static contributor dashboard. It is written for the Coder implementing
`app/index.html` and `app/styles.css`; this handoff does not prescribe a
framework or require client-side interaction beyond loading and presenting the
project data.

## Experience goal

Project Pulse should feel like a calm, useful work overview rather than a
generic landing page. A contributor should be able to answer these questions
in the first view:

1. What is this dashboard?
2. Which projects are active and who owns them?
3. What is each project's current status and priority/risk?
4. What changed recently, and what is the short context for the project?

Keep the page content-first. Do not add decorative charts, controls, or
navigation that are not backed by the data contract. The first viewport must
clearly read as a Project Pulse dashboard even when viewed without JavaScript
enhancements beyond the required data rendering.

## Information hierarchy

Use this order in the page:

1. **Page identity:** a single visible `h1` with the exact text `Project Pulse`,
   followed by a short description such as “A quick view of active projects
   and recent momentum.”
2. **Dashboard summary:** an optional compact line or group of summary facts
   (for example, the number of projects shown). It must not compete with the
   project cards and should be omitted if the available data cannot support an
   accurate value.
3. **Project collection:** a visible section with an `h2` such as “Projects”
   and a responsive collection of one card per project.
4. **Project details:** within each card, prioritize project name, status,
   owner, recent activity, priority/risk, and summary in that order. The name
   and status should be scannable before the supporting details.

Recommended semantic outline:

```html
<main>
  <header class="page-header">...</header>
  <section class="dashboard" aria-labelledby="projects-heading">
    <div class="section-heading">...</div>
    <div class="project-grid">
      <article class="project-card">...</article>
    </div>
  </section>
</main>
```

Use a real `article` for every `.project-card`, a heading element for every
project name, and a `<dl>` for label/value metadata such as owner, recent
activity, and priority. The page should have one `h1`; use `h2` for the
collection heading and a consistent lower-level heading for project names.
Preserve a logical DOM order that matches the visual order. Avoid making the
entire card a link unless the data includes a meaningful destination.

Every card must visibly include:

- project name;
- owner;
- status;
- `recentActivity`;
- priority (or risk treatment);
- a concise summary when supplied by the data.

Give relative or abbreviated activity text an accessible, unambiguous label
when needed (for example, visually showing “2h ago” while exposing the full
date/time in an `aria-label` or adjacent text). Do not hide required project
information in hover-only UI.

## Visual language

Use a warm, neutral page background and a high-contrast surface for cards. A
restrained accent color may identify the product and links, but status and
priority must remain understandable in grayscale and with color vision
differences.

### Layout and hooks

- The primary container **must** use the deterministic `.dashboard` hook.
  Center it with a reasonable maximum width (approximately 1120–1200px) and
  use horizontal padding so content never touches the viewport edge.
- Every project article **must** use the deterministic `.project-card` hook.
- Place cards in a grid (`.project-grid` or equivalent) with equal-width
  columns on wide screens. Cards should stretch to equal row height where
  practical, while each card's content remains naturally readable.
- Use rounded corners, a subtle border, and a restrained shadow. The border
  must remain visible when shadows are unavailable or disabled.
- Keep card actions out of the visual hierarchy unless a real action is
  introduced later; this first version is an overview, not a control panel.

### Status communication

Use a compact status badge near the project name. Each status has:

- a stable text label, such as `On track`, `At risk`, `Blocked`, or
  `Complete`;
- a distinct visual treatment using color plus a non-color cue;
- enough padding and line height to remain legible at normal zoom.

Non-color cues should be implemented with a short status word and, where useful,
a simple text character or CSS-generated shape that is not the sole source of
meaning. Do not use status as only a colored dot, background, or border. Keep
the visible label in the DOM; do not rely on `::before` content for the status
name. Suggested cues are a check for complete, an exclamation for at risk, and
an obstruction/stop cue for blocked, but ordinary text labels are sufficient.

Status colors must meet at least 4.5:1 contrast against their badge background
for normal text. Avoid red/green as the only distinction. Include a clear
focus/reading order that does not require visually matching a badge to a
legend; no legend should be needed to interpret the cards.

### Priority and risk communication

Show priority as explicit text, for example `Priority: High`, `Priority:
Medium`, or `Priority: Low`. Pair it with a consistent non-color treatment:

- **High:** an emphasized label and stronger leading/accent rule;
- **Medium:** a standard label and moderate emphasis;
- **Low:** a quiet label with normal weight.

Do not communicate priority with saturation alone, and do not use tiny icons
without text. If the data uses “risk” rather than priority, use the same
pattern with an explicit `Risk: High` label and a visually distinct but
non-color cue.

## Typography and spacing

Use a system sans-serif stack for predictable rendering and fast loading:
`system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif`.
Typography should be comfortable at 100% browser zoom:

- page title: approximately 2–2.5rem, bold, with a tight but readable line
  height;
- section heading: approximately 1.25–1.5rem;
- project name: approximately 1.1–1.25rem, semibold;
- body and metadata: at least 1rem / 16px;
- badge and supporting labels: never below 0.875rem / 14px.

Use a consistent spacing scale based on 4px or 8px units. Recommended
relationships are 24–32px between the page header and project section, 16–24px
inside a card, 12–16px between card content groups, and 8px between a label and
its value. Keep line length for the introductory copy near 60–70 characters.
Allow text to wrap; never truncate project names, owners, status, activity, or
priority with ellipses in the default view.

## Responsive behavior

Design for a wide desktop first, but keep the content usable from a 320px
viewport through large screens:

- At approximately 900px and above, use a three-column grid when the cards
  remain comfortably readable.
- Between approximately 600px and 899px, use two columns.
- Below approximately 600px, use one column and reduce outer padding rather
  than shrinking type. A single card should occupy the available width.
- At very narrow widths, allow long owner names and activity text to wrap;
  badges may wrap to a second line but must not overflow horizontally.
- Avoid horizontal page scrolling. Use `min-width: 0` on grid/card children
  where needed and allow long data strings to wrap at safe boundaries.
- Preserve the same content order at every breakpoint. Do not move status or
  priority to a hover-only or off-canvas region on mobile.

The layout should remain understandable at 200% browser zoom. Use fluid
container padding and grid gaps, and prefer CSS grid/flex sizing over fixed
card heights. Respect `prefers-reduced-motion: reduce`; the static dashboard
does not need entrance animations, and any future transitions must be removed
or shortened for that preference.

## Accessibility and interaction

- Use `<main>` for the primary content and landmark elements only when they
  clarify structure. The project collection needs an accessible heading.
- Ensure every text/background pair meets WCAG AA contrast: 4.5:1 for normal
  text and 3:1 for large text and meaningful non-text boundaries.
- Use visible labels rather than placeholder-only or icon-only information.
- Do not set `user-select: none`, disable zoom, or use color as the only
  differentiator.
- If cards contain links or buttons, use native elements and give each an
  accessible name that includes the project name. Do not make a non-interactive
  `article` keyboard-focusable.
- Provide a clearly visible `:focus-visible` treatment for every interactive
  element: a 2–3px high-contrast outline with at least 2px offset, not only a
  subtle color change. The focus indicator must remain visible against the
  page and card surfaces.
- Keep keyboard order equal to reading order: page identity, summary (if
  present), section heading, then each card from top left to bottom right.
- Preserve a minimum pointer/touch target of 44px by 44px for any future
  links, buttons, or controls.
- Use `aria-live` only for genuinely asynchronous status changes. Do not mark
  the whole dashboard live during its initial static render.
- Test the visual result with keyboard navigation, browser zoom, grayscale or
  forced-color settings, and a screen reader heading/landmark scan.

For forced-colors or high-contrast environments, ensure borders and text
remain meaningful by using real borders and text labels rather than relying on
box shadows, gradients, or background colors. If CSS-generated status symbols
are used, the adjacent text must still carry the meaning.

## Implementation contract for the Coder

The implementation is expected to:

- reference `styles.css` and `project-data.json` from `app/index.html`;
- render the top-level `projects` array deterministically;
- apply `.dashboard` to the primary dashboard container and `.project-card` to
  every rendered project card;
- keep status and priority text visible in each card;
- use semantic headings, `article`, and metadata labels as described above;
- provide a useful visible loading/error or empty state if data cannot be
  rendered, without pretending that projects loaded successfully.

Recommended class hooks beyond the required ones are `.page-header`,
`.project-grid`, `.status-badge`, `.priority`, `.project-summary`, and
`.project-meta`. These names are guidance, not a requirement, unless the
implementation benefits from them. Avoid styling based on record position or
unlabeled generic selectors when a stable semantic hook is available.

## Acceptance checklist

Before handoff is considered complete, verify that:

- the first viewport visibly presents “Project Pulse” and a project collection;
- every card is a readable, rounded, bordered surface with a visible shadow or
  equivalent separation;
- `.dashboard` and `.project-card` are present and stable in the rendered DOM;
- all required project fields are visible and understandable without color;
- status badges and priority treatments remain distinct in grayscale;
- keyboard focus is obvious and the reading order is logical;
- the layout works at wide, tablet, 320px, and 200% zoom widths without
  horizontal scrolling or clipped text;
- headings, landmarks, labels, and card content are usable with a screen
  reader.
