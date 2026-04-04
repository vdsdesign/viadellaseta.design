# Via della Seta - Complete Animation & Interaction Documentation

Source: https://vds-223c04.webflow.io/
Analyzed: 2026-04-04

## Technology Stack

- **GSAP 3.12.5** + **ScrollTrigger** (custom inline scripts)
- **Webflow IX2** (built-in interaction engine, ~16 action lists)
- **Unicorn Studio v2.0.5** (decorative WebGL canvas effects)
- **jQuery 3.6.0** (used by GSAP client list animation)
- No Lenis (no smooth scroll library detected)
- No page transition library (standard navigation)

## Color Palette (CSS Custom Properties)

- `--beige`: rgb(237, 230, 220) — main background
- `--dark-brown`: rgb(105, 61, 34) — primary text, FAQ active bg
- `--brown`: (used on project cards, tags)
- `--light-brown`: #c9aa7c — accent, marquee text color (at 20% opacity: #c9aa7c33)
- `--green`: (smart home section accent)
- `--dark-green`: (smart home text)
- `--white`: (services section bg)

## Typography

- Display/headings: "De Martega" (custom font), fallback Verdana
- Secondary display: "De Martega Personaluse"
- Body: system sans-serif
- All headings: `text-transform: uppercase`

---

## 1. NAVIGATION (`.menu-wrap`)

### Position & Behavior
- `position: absolute; top: 1.5em; z-index: 3`
- NOT sticky — stays at top of page, scrolls away
- No hide/show on scroll behavior detected
- `width: 100%; padding-left: 2.5em; padding-right: 2.5em`
- `display: flex; justify-content: space-between; align-items: center`

### Structure
- Left: nav links (How it works, Projects, Contact) — anchor links (#how-it-works, #projects, #form)
- Center: logo (SVG)
- Right: language switcher (globe icon + EN/IT/RU links)

### Hover Effects
- Links: `transition: opacity 0.4s` (standard `<a>` tag transition)

### Mobile Menu
- No hamburger menu detected in the HTML — on smaller screens, Webflow likely collapses the menu
- Mobile padding: `1.125em` left/right

---

## 2. HERO SECTION (`.hero-section`)

### Layout
- `margin-top: 9.75em` (to account for nav)
- Background: `linear-gradient(180deg, var(--beige) 67%, white)` on parent
- H1: `font-size: 9em; line-height: 90%; text-transform: uppercase`
- Subtitle: `font-size: 3.75em` (class `.text-60`)
- Two CTA buttons: brown-fill and green-fill

### Hero Image Gallery — Scroll-Triggered Horizontal Spread

**ActionList: `a` ("Hero-img-scroll")**
**Event: `e` — SCROLLING_IN_VIEW**
**Trigger element:** `#75580f91-94fc-574e-478c-15b70547165e` (the `.hero-img-block.rel1`)
**Config:** smoothing: 50, startsEntering: true

Two rows of images (`.hero-img-wrap._1` and `.hero-img-wrap._2`) that spread apart horizontally as you scroll:

| Scroll % | `.hero-img-wrap._1` X | `.hero-img-wrap._2` X |
|-----------|----------------------|----------------------|
| 0% | 0em | 0em |
| 100% | +15em (moves right) | -15em (moves left) |

**Initial CSS positions:**
- `.hero-img-wrap._1`: `position: relative; left: -18.7em` (starts offset left)
- `.hero-img-wrap._2`: `position: relative; right: -0.4em`

**Effect:** As the hero section scrolls through the viewport, the two rows of images spread apart horizontally — left row moves further left, right row moves further right.

**Images in each row (5 per row):**
- Row 1: kitchen interior, decorative element (Group 51), master bedroom, office, master bathroom
- Row 2: office 2, + additional images

**Container:** `.hero-img-block` — `overflow: hidden; display: flex; flex-direction: column; gap: 0.5em; margin-top: 5.75em`

---

## 3. SERVICES/NEEDS SECTION (`.section.white > .needs-wrap`)

### Parallax on Need Images

**ActionList: `a-14` ("need-parallax")**
**Event: `e-69` — SCROLLING_IN_VIEW**
**Trigger:** `.need` section wrapper (`#a704f3f8`)
**Config:** smoothing: 50, startsEntering: true

Each `.need-img` (classical painting images) has:

| Scroll % | translateY | scale |
|-----------|-----------|-------|
| 0% | -1em | 1.1 |
| 100% | +2em | 1.15 |

**Effect:** Images slowly drift downward (3em total travel) while slightly zooming in (1.1 to 1.15 scale). Container `.need` has the image positioned absolutely at `inset: -24% 0% 0% 19.5%` with `overflow: hidden` (not on need itself but clipped by card shape).

**Image sizing:** `width: 16.5em; height: 24em; object-fit: cover`

---

## 4. "FOR PROFESSIONALS" SECTION (`.section-brown`)

### Parallax on Circular Images

**ActionList: `a-15` ("4professionals-parallax")**
**Event: `e-70` — SCROLLING_IN_VIEW**
**Trigger:** `._4professionals-info-wrap` (`#3df300cd`)
**Config:** smoothing: 50, startsEntering: true

Each `._4professionals-img` (circular clipped images):

| Scroll % | translateY | scale |
|-----------|-----------|-------|
| 0% | -0.75em | 1.2 |
| 100% | +0.75em | 1.2 |

**Effect:** Images drift 1.5em vertically while maintaining 1.2x zoom. Container `._4professionals-img-wrap` is `border-radius: 300em` (circle), `overflow: hidden`, `width/height: 6.125em`.

---

## 5. PROCESS CARDS ("How it works") — Scroll-Driven Card Stack

**ActionList: `a-2` ("Process-card-scroll")**
**Event: `e-2` — SCROLLING_IN_VIEW**
**Trigger:** `.process-cards-wrap` (`#f7e1591f`)
**Config:** smoothing: 50, startsEntering: true
**Media queries:** main, medium, small (NOT tiny/mobile)

6 cards that fan out like a hand of playing cards as user scrolls. Each card has a rotation and vertical offset:

### Initial State (0% scroll):
| Card | Class | Rotate Z | Y Offset |
|------|-------|----------|----------|
| 1 | `.process-card._1` | -6deg | 0 |
| 2 | `.process-card.light-green._2` | 6deg | 0 |
| 3 | `.process-card.text-brown-8f6227._3` | -1deg | -5em |
| 4 | `.process-card.text-green-888660._4` | 6deg | 0 |
| 5 | `.process-card.text-light-brown-c9aa7c._5` | (initial) | 0 |
| 6 | `.process-card._6` | 11deg | 0 |

### Keyframe Animation Sequence:

**Card 2** (keyframe 30%): Y = -5em, rotate = 12deg
**Card 5** (keyframe 30%): Y = 0, rotate = -4deg
**Card 3** (keyframe 55%): Y = -15em, rotate = -5deg
**Card 4** (keyframe 70%): Y = -20em, rotate = 12deg
**Card 5** (keyframe 85%): Y = -35em, rotate = -9deg

### Final State (100% scroll):
| Card | Rotate Z | Y Offset |
|------|----------|----------|
| 1 | -12deg | (same) |
| 6 | 18deg | -30em |

**CSS initial rotations (before IX2 takes over):**
- Card 1: `rotate(-6.6deg)`
- Card 2: `align-self: flex-end; rotate(6.5deg)`
- Card 3: `rotate(-1.2deg)`
- Card 4: `align-self: flex-end; rotate(12deg)`
- Card 5: `rotate(-8deg)`
- Card 6: `align-self: flex-end; rotate(16deg)`

**Mobile (tiny):** All rotations set to `none`

**Behind text:** `.process-text-behind` — "How it works" in giant text (21em font-size), `color: #c9aa7c33` (light brown at 20% opacity), `position: sticky; top: 1em; height: 0` (overlaps cards from behind).

### Process Text Disappear Animation

**ActionList: `a-9` ("process-text-dissappear")**
**Event: `e-62` — SCROLLING_IN_VIEW**
**Trigger:** `#d3e90559` (the process section)

| Scroll % | `.process-text-behind` opacity |
|-----------|-------------------------------|
| 70% | 1 |
| 86% | 0 |

---

## 6. SMART HOME SECTION — Ornament Rotation

**ActionList: `a-11` ("ornament")**
**Event: `e-65` — PAGE_START, loop: true**
**Target:** PAGE (homepage)

Continuous infinite rotation of `.smart-ornament` elements:

1. Start: rotateZ(0deg)
2. Animate: rotateZ(360deg) over **60 seconds** (duration: 60000ms)
3. Reset: rotateZ(0deg) instantly (duration: 0)
4. Loop forever

**4 ornament SVGs** positioned absolutely around the smart home section:
- `.smart-ornament` — `width: 35em; height: 35em; position: absolute; top: -14.3em; left: -5em`
- `.smart-ornament._2` — right: -5em
- `.smart-ornament._3` — bottom: -6.5em
- `.smart-ornament._4` — (additional position)

**Gradient overlays:** `.smart-grad-top` and `.smart-grad-btm` — gradient masks at top/bottom

---

## 7. BRAND LOGOS SECTION — Scroll-Driven Marquee (`.section.italian`)

**ActionList: `a-12` ("brands")**
**Event: `e-67` — SCROLLING_IN_VIEW**
**Trigger:** `#730db3ee` (`.section.italian`)
**Config:** smoothing: 50, startsEntering: true

Three rows of brand logos that scroll horizontally at different speeds/directions as user scrolls:

| Scroll % | Row ._1 X | Row ._2 X | Row ._3 X |
|-----------|-----------|-----------|-----------|
| 0% | -100% | 0% | -100% |
| 100% | -30% | -60% | 0% |

**Directions:**
- Row 1 (`.italian-logos-wrap-in._1`): moves RIGHT (from -100% to -30%) — 70% travel
- Row 2 (`.italian-logos-wrap-in._2`): moves LEFT (from 0% to -60%) — 60% travel
- Row 3 (`.italian-logos-wrap-in._3`): moves RIGHT (from -100% to 0%) — 100% travel

**Structure:** Each row contains duplicate sets of brand logos (for seamless effect). Container `italian-alllogos-wrap` has `overflow: hidden`. Each `.italian-logos-wrap-in` has `flex: none; display: flex; gap: 3em`.

**Brands shown:** Baxter, Giorgetti, Henge, B&B Italia, MolteniC, Minotti, Flexform, Poliform (each row duplicated).

**Category label:** "Outdoors" (`.text-caps`)

---

## 8. SELECTED PROJECTS — Scroll-Driven Card Carousel

**ActionList: `a-13` ("Projects-scroll")**
**Event: `e-68` — SCROLLING_IN_VIEW**
**Trigger:** `#d58135f5` (project section wrapper)
**Config:** smoothing: 50, startsEntering: true

Three project cards that fade in/out sequentially as user scrolls through a tall sticky section:

### Opacity Timeline:

| Scroll % | Card ._1 | Card ._2 | Card ._3 |
|-----------|----------|----------|----------|
| 6% | 0 | — | — |
| 10% | **1** | — | — |
| 17% | 1 | — | — |
| 21% | 0 | — | — |
| 30% | — | 0 | — |
| 34% | — | **1** | — |
| 48% | — | 1 | — |
| 52% | — | 0 | — |
| 62% | — | — | 0 |
| 68% | — | — | **1** |
| 79% | — | — | 1 |
| 83% | — | — | 0 |

**Effect:** Cards crossfade sequentially. Each card is visible for roughly 7-18% of scroll, with 4% fade transitions.

**Layout:**
- `.project-blockl`: `width: 100vw; position: relative`
- `.project-card-wrap`: `position: sticky; top: 15.9em; height: 0` — cards stack on top of each other
- `.project-card`: `position: absolute; width: 41.5em; height: 20.25em; z-index: 1`
- Background images: `.project-bg-img-2` — `width: 100vw; height: 60em; position: static; z-index: -1`

**Projects:**
1. Bvlgari Hotel Milano (Milan) — Hotel, Kitchens
2. Villa Caterina in Lucerne — (Lucerne)
3. House on the coast of Como (Bellagio)

**Link:** "Selected projects" button goes to `/casa-quiete`

---

## 9. FAQ ACCORDION (`.clients_wrap`)

### Webflow IX2 Accordion (Desktop — main breakpoint)

**7 FAQ items**, each with the same interaction pattern:

**OPEN (ActionList: `a-5` — "new-OPEN-accordion 2")**
Triggered by: MOUSE_CLICK on `.faq-accordion-trigger`

Initial state (first group, used as initial state):
1. `.svg-nfaq` icon: rotateZ(0deg)
2. `.faq-accordion-panel`: height = 0px

Animation (second group):
1. `.svg-nfaq` icon: rotateZ(180deg) — duration: **200ms**, easing: default
2. `.faq-accordion-panel`: height = AUTO — duration: **200ms**, easing: **outQuad**

**CLOSE (ActionList: `a-6` — "CLOSe-accordion 2")**
Triggered by: MOUSE_SECOND_CLICK

1. `.faq-accordion-panel`: height = 0px — duration: **200ms**, easing: **outQuad**
2. `.svg-nfaq` icon: rotateZ(0deg) — duration: **200ms**

### Webflow IX2 Accordion (Tablet/Mobile — medium, small, tiny)

**OPEN (ActionList: `a-7` — "new-OPEN-accordion-tablet")**

Initial state:
1. Icon: rotateZ(0deg)
2. Background: beige (rgb 237, 230, 220)
3. Panel height: 0px
4. Trigger text: dark-brown
5. Panel text: dark-brown

Animation:
1. Icon: rotateZ(**180deg**) — **200ms**
2. Trigger text: **beige** (rgb 237, 230, 220) — **200ms**
3. Panel: height = **AUTO** — **200ms**, easing: **outQuad**
4. Background (`.client_item`): **dark-brown** (rgb 105, 61, 34) — **500ms**
5. Panel text: **beige** — **200ms**

**CLOSE (ActionList: `a-8` — "CLOSe-accordion-2-tablet")**
1. Panel: height = 0px — **200ms**, easing: **outQuad**
2. Icon: rotateZ(0deg) — **200ms**
3. Background: back to beige — **200ms**
4. Trigger text: back to dark-brown — **200ms**
5. Panel text: back to dark-brown — **200ms**

### GSAP Hover Effect on FAQ Items (Desktop only)

**Custom inline script** — applied to `[data-hover-item]` elements
**Media query:** `(hover: hover) and (pointer: fine)` — desktop only

**Elements:**
- `[data-hover-bg]` = `.nfaq-accordion-bg` — dark brown background overlay
- `[data-hover-content]` = `.client_content` — content wrapper

**Mouse Enter:**
1. Detect cursor Y position relative to item midpoint
2. Set `transformOrigin` to "top" or "bottom" depending on entry direction
3. Animate `.nfaq-accordion-bg` scaleY: 0 -> **1** — duration: **0.4s**, ease: **power2.out**
4. Add class `is-active` to content (changes text color to beige: `.client_content.is-active { color: var(--beige) }`)
5. Animate content paddingLeft/Right to 0rem — duration: **0.4s**, ease: **power2.out**

**Mouse Leave:**
1. Same cursor detection for transformOrigin
2. Animate `.nfaq-accordion-bg` scaleY: 1 -> **0** — duration: **0.4s**, ease: **power2.in**
3. Remove class `is-active`
4. Animate padding back — duration: **0.4s**, ease: **power2.in**

**CSS of `.nfaq-accordion-bg`:**
```css
.nfaq-accordion-bg {
  z-index: 1;
  background-color: var(--dark-brown);
  transform-origin: 50% 0;
  transform-style: preserve-3d;
  width: 100%; height: 100%;
  position: absolute; inset: 0%;
  transform: scale3d(1, 0, 1); /* initially hidden */
}
```

### GSAP Scroll-Triggered Line & Content Reveal

**Custom inline script** on `.clients_wrap`:

For each `.client_list_wrap`:
- **Trigger:** `start: "top bottom"`, `end: "top 70%"`
- **toggleActions:** `"none play none reset"` (play on enter, reset on leave)

Timeline:
1. `.line_full_wrap`: scaleX from **0** to 1 — stagger: **0.05s**, duration: **0.5s**, ease: **power2.out**
2. `.client_content`: opacity from **0** + yPercent from **20** to 0 — stagger: **0.1s**, duration: **0.5s**, ease: **none** (starts simultaneously with lines, position 0)

---

## 10. CONTACT FORM SECTION (`.section.form`)

### "Let's Talk" Marquee

**ActionList: `a-10` ("marquee-form")**
**Event: `e-63` — SCROLL_INTO_VIEW, loop: true**
**Trigger:** `.section.form` (`#3739ea82`)

Infinite horizontal scrolling marquee behind the form:

1. Start: `.marquee-wrap` translateX(**-100%**)
2. Animate: translateX(**+232.75%**) over **60 seconds** (60000ms)
3. Reset: translateX(**-100%**) instantly (duration: 0)
4. Loop infinitely

**Structure:**
- `.marquee-wrap`: `position: absolute; inset: 0%; height: 100vh; z-index: -1; overflow: visible; display: flex`
- Contains 2x `.form-marquee-in` (duplicate for seamless loop), each containing 5x "Let's talk" text
- `.form-marq-text`: `font-size: 56.25em; color: #c9aa7c33; text-transform: uppercase; white-space: nowrap; font-family: De Martega`

**Direction:** LEFT to RIGHT (translateX goes from -100% to +232.75%)

**Container:** `.section.form` has `overflow: hidden; z-index: 2; position: relative; background-color: var(--beige)`

---

## 11. FOOTER (`.footer-section`)

### Sticky Footer
```css
.footer-section {
  background-color: var(--dark-brown);
  color: var(--beige);
  padding-top: 1.25em;
  padding-bottom: 2em;
  position: sticky;
  bottom: 0;
}
```

**Effect:** Footer sticks to bottom of viewport and is revealed as main content scrolls away. On mobile (`@media max-width: 991px`): `position: relative` (not sticky).

### Footer Logo Text
- "via della Seta" in giant typography: `.fh { font-size: 20.875em; line-height: 90% }`
- "single supplier for all" above it

### Back-to-top Link
- Arrow SVG + "Back to top" text — links to `#top`
- `transition: opacity 0.4s` on hover

---

## 12. BUTTON HOVER EFFECTS

### All Buttons
```css
.button { transition: opacity 0.2s; }
a { transition: opacity 0.4s; }
```

### Button Variants
- `.button.brown-fill`: `background-color: var(--dark-brown); color: var(--light-brown); border: none`
- `.button.green-fill`: `background-color: var(--green); color: var(--dark-green); border: none`
- `.button` (outline): `border: 1px solid var(--dark-brown); padding: 1.25em 1.5em; height: 4em`
- All buttons: `display: flex; gap: 0.625em; align-items: center`

---

## 13. CASA QUIETE PROJECT PAGE (`/casa-quiete`)

### Page Structure
- Same navigation as homepage
- `.hero-case` section with `margin-top: 9em`
- `.case-block`: `display: flex; justify-content: space-between; align-items: center`
- `.hero-case-wrap`: `width: 46em; display: flex; flex-direction: column; gap: 0.375em`
- `.project-card.case`: `width: 34.625em; height: 28.75em; padding: 4.625em 2.25em`

### Project Details Cards
- `.project-card.case-btm`: `width: 41.875em; height: 28.75em; padding: 4em 7.5em`
- `.case-cards-wrap`: `display: flex; flex-wrap: wrap; gap: 1.25em`

### "See Also" Section — Scroll-Driven Horizontal Movement

**ActionList: `a-16` ("see-also-scroll")**
**Event: `e-73` — SCROLLING_IN_VIEW**
**Trigger:** The see-also block on the project page

Three rows of project links that shift horizontally:

| Scroll % | Row ._1 X | Row ._2 X | Row ._3 X |
|-----------|-----------|-----------|-----------|
| 0% | -10em | 0em | -10em |
| 100% | 0em | -10em | 0em |

**Directions:**
- Row 1: moves RIGHT (from -10em to 0)
- Row 2: moves LEFT (from 0 to -10em)
- Row 3: moves RIGHT (from -10em to 0)

**Container:** `.case-links-block` has `overflow: hidden`

**Projects linked:** "Penthouse Terrace, Zurich" (and others)
**Text:** `.110-text._80` — large uppercase project names

### "Let's Talk" Marquee on Project Page

**Event: `e-71` — SCROLL_INTO_VIEW, loop: true**
Same marquee animation as homepage (`a-10`), applied to `#54bd08a1` (the form section on this page).

### No Additional Page-Specific Animations
- Only 2 data-w-id elements on this page
- No additional inline GSAP scripts
- Uses the same Webflow IX2 interaction engine

---

## 14. UNICORN STUDIO

Loaded via `unicornStudio.umd.js v2.0.5`. The init() call triggers on DOMContentLoaded. No explicit `data-us-project` attributes found in HTML, but the library is initialized. This likely provides subtle WebGL canvas effects (grain/texture/noise overlays) on certain sections. The exact effect is embedded in Unicorn Studio's config and would need the project ID to replicate — it may be configured through Webflow's designer panel or a hidden data attribute.

---

## 15. PAGE TRANSITIONS

**None detected.** Standard browser navigation between pages. No Barba.js, Swup, or Webflow page transition animations in the code.

---

## 16. MOBILE MENU BEHAVIOR

No dedicated mobile hamburger menu detected in the HTML. The desktop nav links (How it works, Projects, Contact) appear to be hidden or rearranged via CSS breakpoints. The menu-wrap padding changes to `1.125em` on small screens.

---

## Summary of All Animations

| # | Animation | Type | Trigger | Duration/Speed | Easing |
|---|-----------|------|---------|---------------|--------|
| 1 | Hero images horizontal spread | IX2 SCROLLING_IN_VIEW | Scroll through hero | Continuous (0-100% scroll) | Linear |
| 2 | Process cards fan out (6 cards) | IX2 SCROLLING_IN_VIEW | Scroll through process section | Continuous keyframes at 0/30/55/70/85/100% | Linear |
| 3 | Process "How it works" text fade | IX2 SCROLLING_IN_VIEW | Scroll 70-86% of section | Continuous | Linear |
| 4 | Smart home ornament rotation | IX2 PAGE_START, loop | Page load | 60s per rotation | Linear |
| 5 | Brand logos 3-row scroll | IX2 SCROLLING_IN_VIEW | Scroll through brands section | Continuous (0-100%) | Linear |
| 6 | Project cards opacity crossfade | IX2 SCROLLING_IN_VIEW | Scroll through projects | Continuous keyframes | Linear |
| 7 | Need images parallax + zoom | IX2 SCROLLING_IN_VIEW | Scroll through needs | Y: -1em to +2em, scale: 1.1 to 1.15 | Linear |
| 8 | 4professionals image parallax | IX2 SCROLLING_IN_VIEW | Scroll through section | Y: -0.75em to +0.75em, scale: 1.2 | Linear |
| 9 | FAQ accordion open | IX2 MOUSE_CLICK | Click FAQ trigger | 200ms | outQuad (height) |
| 10 | FAQ accordion close | IX2 MOUSE_SECOND_CLICK | Click again | 200ms | outQuad (height) |
| 11 | FAQ hover bg reveal | GSAP inline | mouseenter/leave | 0.4s | power2.out / power2.in |
| 12 | FAQ line + content reveal | GSAP ScrollTrigger | Scroll into view (top bottom -> top 70%) | 0.5s, stagger 0.05-0.1s | power2.out (lines), none (content) |
| 13 | "Let's talk" marquee | IX2 SCROLL_INTO_VIEW, loop | Section enters viewport | 60s per cycle | Linear |
| 14 | See-also links horizontal shift | IX2 SCROLLING_IN_VIEW | Scroll through section (project page) | Continuous 10em travel | Linear |
| 15 | Sticky footer reveal | CSS only | Scroll past content | N/A | N/A |
| 16 | Button/link opacity | CSS transition | Hover | 0.2s / 0.4s | Default (ease) |
