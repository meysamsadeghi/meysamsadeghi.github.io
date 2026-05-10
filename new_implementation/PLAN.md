# Plan: Recreate GitHub Pages Site in `new_implementation/`

## Context

The existing site is a Jekyll academic portfolio using the Academic Pages theme (a heavily customized Minimal Mistakes fork with 121 SCSS files, 44 includes, 8 layouts, and a multi-theme system). The goal is to replicate the same content, structure, and visual appearance as a self-contained plain HTML/CSS static site under `new_implementation/` — no Jekyll, no Ruby, no build step required.

## What the Existing Site Actually Renders

- **Header:** Dark nav bar with site title "Meysam Sadeghi's Homepage" — no nav links (all commented out in `_data/navigation.yml`)
- **Layout:** Two-column: ~280px sticky sidebar on the left + main content on the right
- **Sidebar:** Profile photo (circle), name, bio, email, and social icons (Google Scholar, GitHub, LinkedIn, Twitter)
- **Homepage (`/`):** Bio paragraph → awards list → two publication cards (image + text side-by-side)
- **Other pages:** Publications listing, Talks, Teaching, CV, 404

---

## Directory Structure

```
new_implementation/
├── index.html           # Homepage (about + awards + selected publications)
├── publications.html    # All publications grouped by category
├── talks.html           # All talks, newest first
├── teaching.html        # All teaching entries, newest first
├── cv.html              # Education, work experience, skills
├── 404.html             # Not-found page
├── style.css            # All CSS (no preprocessor)
└── images/
    ├── profile.png      # copied from /images/profile.png
    ├── favicon.ico      # copied from /images/favicon.ico
    ├── gero.jpg         # copied from /assets/images/gero.jpg
    └── AdvAttack.png    # copied from /assets/images/AdvAttack.png
```

All HTML files are at the same level. All paths are relative and flat (`href="style.css"`, `src="images/profile.png"`).

---

## CSS Architecture (`style.css`)

Single file, organized in sections:

| # | Section | Key details |
|---|---------|-------------|
| 1 | Reset & base | `box-sizing: border-box`, `system-ui` font stack, line-height |
| 2 | CSS custom properties | `--header-bg: #1a1a2e`, `--accent: #2563eb`, `--sidebar-width: 280px` |
| 3 | Layout | `.site-body { display: flex }`, sidebar fixed width, main `flex: 1` |
| 4 | Header | Full-width dark bar, site title left-aligned as home link |
| 5 | Sidebar | Sticky, avatar circle (150px, `border-radius: 50%`), name, bio, social links |
| 6 | Main content | Padding, `h2` with bottom border for section headings |
| 7 | Publication cards | `.pub-card { display: flex }`, image 280px wide, text alongside |
| 8 | Archive items | `.archive-item` with bottom border for publications/talks/teaching pages |
| 9 | CV sections | `.cv-entry` with left accent border |
| 10 | Footer | Centered, small muted text |
| 11 | Responsive | Single breakpoint at 768px: sidebar stacks above main, pub cards stack vertically |

**Single external dependency:** Font Awesome 6.5 via CDN for social icons (`fa-envelope`, `fa-graduation-cap` for Scholar, `fa-github`, `fa-linkedin`, `fa-x-twitter`).

---

## Page Content

### `index.html`
- Bio paragraph (Qualcomm, physical AI, autonomous driving)
- PhD background (SUTD, best PhD dissertation award; postdoc at Linköping University)
- **Selected Awards and Grants** (7 items, 2013–2025)
- **Selected Publications** — two `.pub-card` entries:
  1. GeRo paper — `images/gero.jpg` + link to arXiv + description
  2. Adversarial Attacks paper — `images/AdvAttack.png` + link to arXiv + description

### `publications.html`
Google Scholar link at top. Grouped by category with `<h2>…</h2><hr>`:

**Journal Articles** (newest first):
- Paper 3 (2015) — Journal 1, with slides PDF
- Paper 2 (2010) — Journal 1, with slides PDF
- Paper 1 (2009) — Journal 1, with slides, paper, and bibtex PDFs

**Conference Papers** (newest first):
- Paper 5 (2024) — title: "Paper Title Number 5, with math E=mc²" *(Unicode `²` replaces MathJax `$$E=mc^2$$`)*
- Paper 4 (2024) — "Paper Title Number 4"

Each entry: title, venue/year, excerpt, and download links where available.

### `talks.html`
4 talks, newest first:
1. Conference Proceeding talk 3 — Testing Institute of America, LA, March 2014
2. Talk 2 — London School of Testing, London, Feb 2014
3. Tutorial 1 — UC-Berkeley Institute for Testing Science, Berkeley, March 2013
4. Talk 1 — UC San Francisco, Department of Testing, SF, March 2012

### `teaching.html`
2 entries, newest first:
1. Teaching experience 2 — Workshop, University 1, 2015
2. Teaching experience 1 — Undergraduate course, University 1, 2014

### `cv.html`
- **Education:** PhD (Version Control Theory, GitHub University, 2018), MS (Jekyll, 2014), BS (GitHub, 2012)
- **Work Experience:** 3 positions (Spring 2024 Collaborator; Fall 2015 Research Assistant; Summer 2015 Research Assistant)
- **Skills:** Skill 1, Skill 2 (with sub-skills 2.1–2.3), Skill 3

### `404.html`
Title "Page Not Found", message "Sorry, but the page you were trying to view does not exist.", link back to homepage.

---

## Implementation Steps

1. `mkdir -p new_implementation/images`
2. Copy images:
   - `cp images/profile.png new_implementation/images/`
   - `cp images/favicon.ico new_implementation/images/`
   - `cp assets/images/gero.jpg new_implementation/images/`
   - `cp assets/images/AdvAttack.png new_implementation/images/`
3. Write `new_implementation/style.css`
4. Write `new_implementation/index.html`
5. Write `new_implementation/publications.html`
6. Write `new_implementation/talks.html`
7. Write `new_implementation/teaching.html`
8. Write `new_implementation/cv.html`
9. Write `new_implementation/404.html`

---

## Verification

Open `new_implementation/index.html` directly in a browser (`file://` protocol) and check:

- [ ] Two-column layout: sidebar left, main content right
- [ ] Profile photo renders as a circle
- [ ] Social icons appear (requires internet for Font Awesome CDN)
- [ ] Bio text, awards list, and publication cards with images display correctly
- [ ] Responsive: at ≤768px viewport, sidebar stacks above content; pub cards stack vertically
- [ ] Navigate to each page (`publications.html`, `talks.html`, `teaching.html`, `cv.html`) — consistent header/sidebar/footer
- [ ] `404.html` renders and links back to homepage

No build step or local server required — all files work when opened directly in a browser.

---

## Source Files Referenced

| File | Purpose |
|------|---------|
| `_pages/about.md` | Homepage content (bio, awards, selected publications) |
| `_config.yml` | Author info, social links, publication categories |
| `_data/navigation.yml` | Navigation (all links commented out) |
| `_publications/*.md` | 5 publication entries |
| `_talks/*.md` | 4 talk entries |
| `_teaching/*.md` | 2 teaching entries |
| `_pages/cv.md` | CV content |
