# Labels in a post (exploring interactions)

Interaction prototypes for the labels section of an evaluation post in
**Influence Monitor**, a social-media influencer and brand monitoring product.

They exist to answer one question:

> Is it better to resolve label changes **in place on the page**, or **inside a
> modal**?

Handling a second person editing the same post is one of the tests that
separates the two candidates. It is not the goal in itself.

A fifth page, **the admin**, sits beside them: where the label catalogue itself
is managed. It answers a different question — see [The admin](#the-admin).

Each prototype is a working page, not a picture of one — open it and use it as
you would the real product. The dark bar across the top of v2, v3 and v4 is
facilitator scaffolding for interviews, not product UI.

## The prototypes

Open `index.html` for the cover, or go straight to one:

| | | |
| --- | --- | --- |
| **admin v2** | [`[admin v2] Labels and categories.dc.html`](<[admin v2] Labels and categories.dc.html>) | **New.** The admin after the first round of feedback; see [Admin v2 — what changed](#admin-v2--what-changed). |
| **admin v1** | [`[admin] Labels and categories.dc.html`](<[admin] Labels and categories.dc.html>) | Managing the catalogue — categories and their labels — laid out like the product's Keyword Groups screen. Not part of the in-place vs modal question; see [The admin](#the-admin). |
| **v4** | [`[v4] Save and create, in place.dc.html`](<[v4] Save and create, in place.dc.html>) | **Current — used in interviews.** v3's winning candidate — everything resolved in place on the page — on its own, with the ability to create new labels (and, via "Category / New label", new categories) restored to the same combobox that already adds existing ones. |
| **v3** | [`[v3] Save flow - in place vs modal.dc.html`](<[v3] Save flow - in place vs modal.dc.html>) | The two candidates side by side. **A** resolves every action in place on the page, so the card itself is the editor; **B** resolves everything inside an "Edit labels" modal. Both share the same rules underneath — an explicit save step, visible authorship and timestamp on each label, and a switch to simulate someone else editing the same post — so a preference comes from the interaction model, not from a feature one has and the other lacks. |
| **v2** | [`[v2] Input variants a-h.dc.html`](<[v2] Input variants a-h.dc.html>) | The exploration that narrowed the field: eight ways (a–h) of adding and managing labels, from fully inline to fully modal. v3 takes the two ends of that range and makes them comparable. |
| **v1** | [`[v1] Section mockups.dc.html`](<[v1] Section mockups.dc.html>) | Where it started: static mockups of the labels section on its own, before any interaction was decided. |

Two numbering schemes run side by side and mean different things. **v1/v2/v3/v4**
number the prototypes in this repository, oldest to newest. **A/B** name the two
candidates *inside* v3. They were once called V1 and V2, which collided with the
file numbering; v3's state keys in the source are still `'v1'` and `'v2'`, and
the section comments name the letter each one maps to. v4 forks only v3's
candidate A, so it has no A/B split to collide with — its source keeps a single
`state.draft`, not a per-variant key.

## Two rules v3 encodes

**Categories are context, not something you pick.** Every label still shows
the category it lives under — as a "Brand /" prefix on its chip, and as a
group header when you're adding one — but neither candidate lets you apply a
whole category to a post. A category header in the menu (A) or the modal (B)
is inert: it groups the labels beneath it and nothing more. You add and
remove labels one at a time. (v3 originally let a category go on as one unit,
covering its members; that model's supporting code — the "covered" styling,
the read-only category viewer — is still here, just unreachable from either
picker now that adding a whole category isn't offered.)

**Nothing is created on this screen.** New labels come from elsewhere in the
product, so neither candidate offers a "Create …" option; a search that
matches nothing simply says so.

**Every saved label says who added it, and when.** The card chip's popover
has always carried both; the "Edit labels" modal's list now does too — a
label added by someone else reads "Added by Maria Silva, three days ago"
rather than just naming her.

## What v4 changes

v4 forks v3's candidate **A** only — a single mode, resolved in place on the
page. Candidate B, the "Edit labels" modal, the A/B switcher, and the
already-dead whole-category-as-a-unit machinery (the read-only category
viewer, the "Covered" chip state) are all dropped, not just hidden.

It also reverses v3's second rule above: **labels and categories can be
created again**, from the same in-place combobox that already adds existing
ones. Typing a plain name stages a new, unassigned label; typing `Category /
New label` stages the label inside that category, creating the category too
if it doesn't exist yet. Nothing is created until you save — a "Create …" row
only stages the intent, the same way picking an existing label does, and
`commit()` is what turns it into a real catalog entry on Save. There is no
bare/empty-category creation: a category only ever comes into being as a side
effect of creating a label under it. The first rule is unchanged — a category
is still context, not something you apply as a whole.

## The admin

`[admin] Labels and categories.dc.html` is the other side of v4. v4 lets anyone
create a label, or a category, straight from a post; the admin is where the
catalogue that produces gets looked after. It mirrors the product's Keyword
Groups screen — keyword group → **category**, keyword → **label** — and uses the
same design system.

Four views, each with its own URL so the browser's Back button works:

| View | What it shows |
| --- | --- |
| **Categories** (`#/categories`) | The Keyword Groups list, for categories: labels and posts per category, created and last edited. Labels without a category sit in a pinned "No category" row after the last page — it is not a category, so it can't be selected, renamed or deleted. |
| **All labels** (`#/labels`) | Every label in one flat, sortable table, with its category. For clean-up across categories: search, sort by posts, "0 posts only", rename and move in bulk. |
| **A category** (`#/category/1`) | Its labels, with the category's own rename and delete. |
| **A label** (`#/label/1`) | Every post that carries it, who added it and when. The Real Madrid post opens in v4. |

Rules it encodes:

- **Different categories mean different things.** Coca-Cola the drink is not
  Coca-Cola the brand, so labels in different categories are never merged. Merge
  is only offered when every selected label is in the same category. Two
  spellings across categories are fixed by renaming them to one spelling, each
  keeping its own category and posts.
- **A collision inside one category is a merge.** Renaming or moving a label onto
  a name that already exists in that category offers a merge instead of creating
  a duplicate — which is how a "nike" created on a post without a category ends
  up as Brand / Nike.
- **Duplicating makes an independent copy.** It starts with no posts; linking it
  to the same posts as the original is an opt-in checkbox.
- **Deleting asks what happens next.** A category: keep its labels without a
  category, move them to another one, or delete them too. A label: remove it from
  its posts, or replace it with another label from the same category. Both show
  the affected posts first.
- **Every change says what it touches before it happens**, and every change can
  be undone from the snackbar that follows it (or with `Z`). Changes are
  immediate for everyone — there is no save bar here, unlike on a post.
- **Creating is one field, and it accepts what it says it accepts.** "Add labels"
  follows Keyword Groups' dialog — type, add, see the list build up — but the
  separators its help text names actually work: a typed comma or Enter adds the
  entry, a pasted list splits on commas, new lines or tabs, and Backspace in the
  empty field brings the last entry back to edit. (Keyword Groups says "separated
  by comma" and then blocks the comma key.) Whatever is still in the field when
  you save is saved too. v4's `Category / Label` syntax works, and the list says
  per entry what will be created and what is skipped. Unlike on a post, a
  category can be created empty here.
- **On a category or a label, the breadcrumb is the title** — "Labels › Brand ›
  Real Madrid" — as in Keyword Groups.

Export writes the whole catalogue as Excel (.xlsx), Word (.docx) or JSON. The
files are built in the page, with no library loaded — .xlsx and .docx are
written as zip archives by hand — so the export works offline.

`window.__labelsAdmin` exposes the page's component so an automated check can
read its data and assert the invariants above.

### Admin v2 — what changed

`[admin v2] Labels and categories.dc.html` is a copy of the admin with the first
round of feedback applied. The first admin is kept as it was, for comparison.

- **Categories are deleted one at a time.** With two or more selected, Delete is
  disabled ("Delete one category at a time"); Merge still works on several. A
  little friction is intended: deleting a category is the heaviest change here.
- **A post is known by its picture, not by a name.** Most posts have no title —
  only YouTube videos do — so every list of posts leads with a thumbnail. The
  caption is no longer shown, nor searched.
- **Lists of posts follow the product's Violations page**, at least in their first
  columns: Preview · Creator · Posting time · Platform, then the label's own
  Label added by · Added. The order may still change.
- **Platform reads as the product writes it** — "IG: Story", "YT: Short" — and
  keeps its platform colour. Each sample post now has a format, as on the post
  page ("Instagram Post").
- **A creator's avatar is neutral.** A creator posts on several platforms, so the
  avatar never takes a platform's colour.
- The same thumbnail and "IG: Story, date" line appear in the lists of posts
  inside the delete and merge dialogs.

The sample posts' thumbnails are stock photos from [Unsplash](https://unsplash.com),
used under the [Unsplash License](https://unsplash.com/license), in
`assets/posts/`, chosen to match each post's caption and to avoid prominent
brand marks. Photographers: Igor Batista, Anna Sullivan (matchday); H&CO, Sou
Jest (drop); Harrison Qi, Mykyta Kravčenko (city); Jess Bailey, Anastasiia
Chepinska (giveaway); Fachry Zella Devandra (boots); Alex Simpson, Abigail
Keenan (night); Steve Pancrate, runda choo (training); Alex Saks, Doug Bagg
(gym); Haupes, Kat Sylvester (ad); Shihab Chowdhury, Clay Banks (weekend);
Szabo Viktor, Marissa Lewis (backstage); mr lee, Avtar Singh (kit).

## Where it lives

Published at **<https://vicentesarmento-deus.github.io/im-labels_system/>**.

Every push to `main` redeploys it, via `.github/workflows/pages.yml`. That
workflow assembles the site into `_site` so `.git` and the workflow itself are
not published, and carries `.nojekyll` through — which is what stops a Jekyll
build from excluding the underscore-prefixed `_ds/` directory the entire design
system lives in.

## Running it locally

Opening `index.html` straight from disk works — the pages are built to run
from `file://`, and the design system loads by relative path.

To serve them over HTTP instead:

```
python3 -m http.server 4175
```

Then open <http://localhost:4175/>. A matching launch configuration lives in
`.claude/launch.json`.

**An internet connection is required.** v1–v4 and both admins load React and Babel from unpkg at
runtime, and the design system pulls the Epilogue title font from Google Fonts.
Offline, the prototypes will not boot and headings fall back to Rubik.

## What is in here

```
index.html                  The cover
[v1|v2|v3|v4] ….dc.html     The prototypes
[admin] ….dc.html           The label catalogue admin, v1
[admin v2] ….dc.html        The admin after the first round of feedback
assets/posts/               Stock thumbnails for admin v2's sample posts
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
