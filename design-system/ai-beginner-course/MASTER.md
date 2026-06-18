# AI Beginner Course Design System

> Source of truth for the course site's visual language.
> When changing the UI, read this file first, then check `pages/` for page-specific overrides.

Project: AI Beginner Course
Style: Retro-Pixel Street Style / Pixel Brutalism
Current implementation source: `scripts/generate_lessons.py` CSS block
Generated outputs: `lessons/`, `chapters/`, `site/`, `github-pages-publish/`

## Core Principle

The course teaches hard technical content to beginners, so the UI should feel energetic and memorable without hurting reading comfort.

Use a light paper-like background for long reading. Put the street/pixel attitude into orange buttons, yellow stickers, black borders, hard shadows, pixel number icons, and short tactile interactions.

Do not return to soft SaaS styling: no teal-first palette, no blurred card shadows, no glassmorphism, no large rounded cards, no gradient-heavy surfaces.

## Color Tokens

| Role | Token | Hex | Usage |
| --- | --- | --- | --- |
| Background | `--canvas` | `#F7F7F5` | Whole-page background |
| Surface | `--surface` | `#FFFFFF` | Cards, panels, content blocks |
| Brand Orange | `--primary` | `#FF5C00` | Primary buttons, active panels, strong accents |
| Street Yellow | `--amber` | `#FFD600` | Stickers, labels, secondary buttons |
| Solid Black | `--ink` / `--line` | `#121212` | Text, borders, hard shadows |
| Body Text | `--muted` | `#3D3D3D` | Paragraphs and supporting text |

Implementation rule: use flat colors only. Do not introduce decorative gradients for page backgrounds, cards, or hero art.

## Typography

| Area | Font Stack | Rule |
| --- | --- | --- |
| Display headings, labels, nav brand | `Silkscreen`, `Press Start 2P`, `Zpix`, `Fusion Pixel`, fallback Chinese sans | Use for short titles and stickers only |
| Body copy | `Noto Sans SC`, `PingFang SC`, `Microsoft YaHei`, sans-serif | Use for all long Chinese reading |
| Code/prompt snippets | `Consolas`, `SFMono-Regular`, monospace | Keep readable and scannable |

Do not use pixel fonts for dense paragraphs, worksheet instructions, long lists, or table content.

## Shape And Shadow

All interactive and framed UI should use the same physical rule:

```css
border: 3px solid #121212;
border-radius: 4px;
box-shadow: 5px 5px 0 #121212;
```

Buttons and small stickers use:

```css
box-shadow: 3px 3px 0 #121212;
```

Hover/press interaction:

```css
transform: translate(2px, 2px);
box-shadow: 1px 1px 0 #121212;
transition: transform 0.1s ease, box-shadow 0.1s ease;
```

Avoid soft blur shadows such as `0 14px 30px rgba(...)` unless it is inside a modal/lightbox overlay where the page is dimmed.

## Component Rules

### Buttons

- Primary action: orange background, black text, black border, hard shadow.
- Secondary action: yellow background, black text, black border, hard shadow.
- Use full-width buttons on mobile when stacked in toolbars.
- Every clickable element must visibly react on hover/focus.

### Cards And Panels

- White background.
- 3px black border.
- 5px hard black shadow.
- 4px radius.
- Cards can contain labels, titles, copy, lists, and actions.
- Do not nest decorative cards inside other decorative cards. Inner blocks should be flatter unless they need strong emphasis.

### Stickers And Badges

- Yellow or orange background.
- Black border and hard shadow.
- Slight rotation between -1.2deg and 1deg is allowed.
- Use for chapter labels, track labels, case status, tags, and meta chips.
- Keep sticker text short.

### Lesson Number Icons

- Use the existing orange/black pixel PNG assets in `number-icons/`.
- Render with `image-rendering: pixelated`.
- Add a small black drop shadow.
- Do not replace these with soft circular badges.

### Worksheets

- Writing boxes should look printable and tactile.
- Use dashed black borders and subtle ruled-paper background.
- Do not auto-save; the current course model expects learners to copy useful answers into their own notes.

### Progress Bars

- White track with black border.
- Orange filled segment.
- No blue/green gradient.

### Top Navigation

- Sticky white bar.
- Black border/shadow.
- On mobile, nav links scroll from the left. Do not right-align scrollable nav links because it hides the first link.

## Layout Rules

- Max content width: `1160px`.
- At wide desktop widths, keep the readable center column but use both outer gutters for sticky learning rails. These rails should feel like course guidance panels, not ads or decorative filler.
- At `1660px` and above, the center column may expand to `1280px` so course maps and card grids use more of the screen while long reading stays organized by existing columns and side panels.
- Mobile breakpoint must work at 375px width.
- Use full-width stacking for cards and toolbars on mobile.
- Hide wide learning rails below the desktop breakpoint; do not make mobile users scroll past duplicated side guidance before the lesson content.
- `body { overflow-x: hidden; }` is allowed because hard shadows can extend a few pixels beyond the viewport; content itself still must not overflow.
- Hero should show immediate course action, not marketing-only copy.

## Current CSS Anchor

The generated site's visual system starts at the `CSS = """` block in `scripts/generate_lessons.py`.

Important implementation markers:

- `@import` includes `Noto Sans SC` and `Silkscreen`.
- `:root` defines the canonical tokens.
- The comment `Retro-pixel street style: high contrast, hard edges, sticker labels.` starts the theme override section.
- `sync_assets()` copies `design-system/` into generated publish targets.

## Page Override Lookup

Check these files before changing a page family:

- `pages/course.md` for course home, course system page, case library, and resource hub.
- `pages/lesson-and-worksheet.md` for lesson pages, worksheet pages, and chapter guide pages.

If a new page family is added, create a page-specific override in `pages/` before implementing broad CSS changes.

## Anti-Patterns

- Teal/blue SaaS palette as the dominant identity.
- Large rounded rectangle cards.
- Soft blurred shadows.
- Glassmorphism.
- Decorative gradient orbs/blobs.
- Emoji as UI icons.
- Pixel font for long body text.
- Buttons without hard press feedback.
- Labels without border/shadow.
- Mobile nav that starts scrolled away from the first link.

## Pre-Delivery Checklist

- [ ] `py -3 -m py_compile scripts/generate_lessons.py`
- [ ] `py -3 scripts/generate_lessons.py`
- [ ] Root, `site/`, and `github-pages-publish/` outputs are synchronized.
- [ ] Published HTML contains `Retro-pixel street style`.
- [ ] `number-icons/01.png` resolves in the publish target.
- [ ] Browser check at desktop width and 390px mobile width.
- [ ] No horizontal scroll on mobile.
- [ ] Top nav first link is visible on mobile.
- [ ] Primary button background is `rgb(255, 92, 0)`.
