# Labels in a post (exploring interactions)

Interaction prototypes for the labels section of an evaluation post in
**Influence Monitor**, a social-media influencer and brand monitoring product.

They exist to answer one question:

> Is it better to resolve label changes **in place on the page**, or **inside a
> modal**?

Handling a second person editing the same post is one of the tests that
separates the two candidates. It is not the goal in itself.

Each prototype is a working page, not a picture of one — open it and use it as
you would the real product. The dark bar across the top of v2 and v3 is
facilitator scaffolding for interviews, not product UI.

## The prototypes

Open `index.html` for the cover, or go straight to one:

| | | |
| --- | --- | --- |
| **v3** | [`[v3] Save flow - in place vs modal.dc.html`](<[v3] Save flow - in place vs modal.dc.html>) | **Current — used in interviews.** The two candidates side by side. **A** resolves every action in place on the page, so the card itself is the editor; **B** resolves everything inside an "Edit labels" modal. Both share the same rules underneath — an explicit save step, visible authorship on each label, and a switch to simulate someone else editing the same post — so a preference comes from the interaction model, not from a feature one has and the other lacks. |
| **v2** | [`[v2] Input variants a-h.dc.html`](<[v2] Input variants a-h.dc.html>) | The exploration that narrowed the field: eight ways (a–h) of adding and managing labels, from fully inline to fully modal. v3 takes the two ends of that range and makes them comparable. |
| **v1** | [`[v1] Section mockups.dc.html`](<[v1] Section mockups.dc.html>) | Where it started: static mockups of the labels section on its own, before any interaction was decided. |

Two numbering schemes run side by side and mean different things. **v1/v2/v3**
number the prototypes in this repository, oldest to newest. **A/B** name the two
candidates *inside* v3. They were once called V1 and V2, which collided with the
file numbering; the state keys in the source are still `'v1'` and `'v2'`, and
the section comments name the letter each one maps to.

## Where it lives

Published at **<https://vicentesarmento-deus.github.io/im-labels_system/>**,
deployed from `main` by `.github/workflows/pages.yml` on every push.

## Running it locally

Opening `index.html` straight from disk works — the pages are built to run
from `file://`, and the design system loads by relative path.

To serve them over HTTP instead:

```
python3 -m http.server 4175
```

Then open <http://localhost:4175/>. A matching launch configuration lives in
`.claude/launch.json`.

**An internet connection is required.** v1–v3 load React and Babel from unpkg at
runtime, and the design system pulls the Epilogue title font from Google Fonts.
Offline, the prototypes will not boot and headings fall back to Rubik.

## What is in here

```
index.html                  The cover
[v1|v2|v3] ….dc.html        The prototypes
support.js                  Runtime for the .dc.html pages
_ds/monitoring-design-…/    The Monitoring Design System — tokens, fonts, components
uploads/                    Reference material: screenshots of the real staging page
dist/                       An earlier build, kept for reference; not linked from the cover
.nojekyll                   Stops a GitHub Pages build from excluding the _ds/ folder
```

## Design system

Everything is built on the **Monitoring Design System** in `_ds/` — see its own
`readme.md` for scope and caveats. In short: Material UI's Figma kit with
Influence Monitor's theme layered on top. Primary is IM Blue `#1C00B7`, type is
Rubik for body and Epilogue for display, spacing is on an 8px base with a 4px
half-step, and the radius is a flat 4px.

The cover deliberately departs from that radius: it uses none at all, no
shadows, nothing centred, and 1px rules for every separation. Colour, type and
spacing are still design system tokens throughout.
