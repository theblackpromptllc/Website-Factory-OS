BUILD SPEC — Rooted Hair Studio (Durham, NC)
Agent: Build Spec Agent v1 (Website Factory OS)
Date of run: 2026-06-04
Inputs read in full:
- projects/rooted-hair-studio/copy-output.md (Copy Agent v1, 2026-06-04)
- projects/rooted-hair-studio/strategy-output.md (Strategy Agent v1, 2026-06-03)
- projects/rooted-hair-studio/research-output.md (Research Agent v3, 2026-06-03)

PURPOSE: This is a complete, buildable specification for a static website (semantic HTML +
CSS + a little vanilla JavaScript, no framework). It does NOT build the site — that is a
separate step (see §8). Another agent must be able to execute this without guessing.

NON-NEGOTIABLES CARRIED FROM THE PIPELINE:
- NO FABRICATION. Every [OWNER TO SUPPLY: ...] placeholder from copy-output.md is carried
  straight through into the build, visible on the page. No price, address, phone, hour,
  rating, review, credential, or tenure is invented. No competitor data is borrowed.
- NO review/rating schema (aggregateRating, Review) anywhere — Rooted has no real reviews
  yet (Copy p.124–128, Strategy §4 NOTE, §9 A). Inventing them is fabrication AND violates
  Google structured-data policy.
- CULTURAL LENS governs imagery, design, and language: real photos of type 3–4 hair and
  locs only (never stock or straight-hair); premium and warm, built for her; her exact
  vocabulary in all alt text, titles, meta, and schema; avoid every term the strategy
  flagged as "not for us" ("dreads," "blowout," "hair straightening," "relaxer/silkout"
  as a selling point, "all hair types welcome").


================================================================
1. STACK AND SETUP
================================================================

STACK:
- Hand-authored static HTML5, one file per page (no build tooling, no framework, no bundler).
- One shared CSS file: css/styles.css (the full design system; a single file is enough at
  this scale).
- One shared JS file: js/main.js (vanilla, dependency-free, progressive enhancement).
- Google Fonts via <link> in <head> (default pairing in §3; swap to brand fonts if supplied).
- No external JS libraries. No analytics by default (owner may add later — Open Items).

OUTPUT LOCATION:
- Build everything into projects/rooted-hair-studio/site/.
- The site is fully static and can be served from any static host or opened from the file
  system for review.

GLOBAL CONVENTIONS:
- UTF-8, <html lang="en">, mobile-first responsive, single <h1> per page.
- Every page includes: identical <head> boilerplate (charset, viewport, fonts, CSS link),
  identical header markup, identical footer markup, then a JSON-LD block (global +
  page-specific), then <script src="js/main.js" defer> before </body>.
- The header and footer are HARD-CODED into every HTML file (not injected by JS), so the
  site is fast and fully crawlable. They must be byte-identical across pages (see §3
  Components and §4 Shared Header/Footer).
- Relative links only (e.g., href="locs.html"), so the site works from any path.
- All interactive JS is progressive enhancement: every page must work fully with JS disabled
  (nav links are real <a>s, portfolio shows all photos, FAQ answers are visible, booking is a
  real link/embed).

CANONICAL BUSINESS ENTITY (use verbatim everywhere — name, one-liner, location, services;
this is the AI-SEO "unmistakable entity" requirement, Strategy §6, Research §5):
- Name: Rooted Hair Studio
- One-line description: "A natural-hair studio in Durham, NC specializing in type 3–4 hair
  and locs — locs, silk press, protective styles, and scalp & root health."
- Location: Durham, North Carolina (street address/ZIP = [OWNER TO SUPPLY])
- Services: Locs (starter, retwist & maintenance, extensions); Silk Press (+ treatment);
  Protective Styles (knotless braids, box braids, twists, faux locs); Scalp & Root Health
  (deep conditioning, scalp care).


================================================================
2. SITE STRUCTURE (FILE TREE)
================================================================

projects/rooted-hair-studio/site/
├── index.html                  # PAGE 1  Homepage            (priority HIGHEST)
├── book.html                   # PAGE 2  Book Now             (priority HIGHEST)
├── locs.html                   # PAGE 3  Locs                 (priority HIGH)
├── silk-press.html             # PAGE 4  Silk Press           (priority HIGH)
├── protective-styles.html      # PAGE 5  Protective Styles    (priority HIGH)
├── scalp-root-health.html      # PAGE 6  Scalp & Root Health  (priority MEDIUM-HIGH)
├── portfolio.html              # PAGE 7  Portfolio / Gallery  (priority HIGH)
├── reviews.html                # PAGE 8  Reviews              (priority HIGH)
├── about.html                  # PAGE 9  About / The Studio   (priority MEDIUM)
├── pricing.html                # PAGE 10 Pricing              (priority HIGH)
├── contact.html                # PAGE 11 Contact / Visit      (priority MEDIUM-HIGH)
├── robots.txt
├── sitemap.xml
├── css/
│   └── styles.css
├── js/
│   └── main.js
└── assets/
    ├── images/                 # all owner-supplied photos land here (see §3 Imagery)
    │   └── .gitkeep            # placeholder so the empty folder exists in the build
    └── icons/                  # inline SVGs preferred; this folder for any file-based icons

FILE-NAMING maps 1:1 to the Strategy §7 sitemap. index.html is the homepage; every other
page is a clear, hyphenated, lowercase filename matching its service/topic for clean URLs and
service+city long-tail SEO (Research §5 #1).


================================================================
3. DESIGN SYSTEM
================================================================

DESIGN INTENT (Cultural Lens, Strategy §5 voice): premium and warm, earthy and rooted, made
for HER — not clinical, not generic, not a general-market spa. Confident, calm, editorial.
Generous whitespace, real textured-hair photography as the hero of the visual language, warm
earth tones that read "natural / for us." Mobile-first, because this traffic is mostly mobile
(Research §5 #3).

--- 3.1 TYPE ---

DEFAULT PAIRING (Google Fonts — use unless owner supplies brand fonts; if brand fonts exist,
mark [OWNER TO SUPPLY: brand heading font] / [OWNER TO SUPPLY: brand body font] and swap):
- Headings / display: "Fraunces" (warm, high-contrast modern serif — premium and editorial,
  reads as crafted and human, not corporate). Weights 400, 500, 600; optical sizing on.
- Body / UI: "Inter" (clean, highly legible sans for body, labels, buttons). Weights 400,
  500, 600.

<head> font load (place identically on every page):
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Fraunces:opsz,wght@9..144,400;9..144,500;9..144,600&family=Inter:wght@400;500;600&display=swap" rel="stylesheet">

TYPE SCALE (fluid, mobile-first; defined as CSS custom properties on :root). Body base is
1.0625rem (17px) on mobile rising to 1.125rem (18px) on larger screens for comfortable
reading. Major-third-ish scale with clamp():
  --font-heading: "Fraunces", Georgia, "Times New Roman", serif;
  --font-body: "Inter", system-ui, -apple-system, Segoe UI, Roboto, sans-serif;
  --fs-base:  clamp(1.0625rem, 1.02rem + 0.2vw, 1.125rem);   /* body */
  --fs-sm:    0.875rem;                                       /* fine print, labels */
  --fs-lg:    clamp(1.1875rem, 1.1rem + 0.4vw, 1.375rem);     /* lead paragraphs, subheads */
  --fs-h3:    clamp(1.375rem, 1.25rem + 0.6vw, 1.625rem);
  --fs-h2:    clamp(1.75rem, 1.5rem + 1.1vw, 2.5rem);
  --fs-h1:    clamp(2.25rem, 1.8rem + 2.2vw, 3.5rem);
  --lh-tight: 1.12;   /* headings */
  --lh-body:  1.6;    /* body */
  --tracking-tight: -0.01em;  /* large headings */
RULES: h1 uses --fs-h1 + Fraunces 500, --lh-tight, --tracking-tight. h2 --fs-h2 Fraunces 500.
h3 --fs-h3 Fraunces 600. Body --fs-base Inter 400 --lh-body. Lead/subhead --fs-lg Inter 400,
color --text-muted. Measure (line length) capped at 65ch for body text blocks.

--- 3.2 COLOR ---

DEFAULT PALETTE (warm, premium, accessible; use unless owner supplies brand colors — if so,
mark each [OWNER TO SUPPLY: brand <role> color] and substitute, then re-check contrast).
Roles as CSS custom properties on :root:
  --bg:            #FAF5EF;  /* warm ivory — page background */
  --bg-alt:        #F1E7DB;  /* warm sand — alternating section bands */
  --surface:       #FFFFFF;  /* cards */
  --text:          #241C17;  /* espresso — primary text (on --bg ≈ 13:1, AAA) */
  --text-muted:    #5A4D43;  /* warm taupe — secondary text (on --bg ≈ 6.4:1, AA) */
  --primary:       #9C3D26;  /* deep terracotta/clay — primary action (white text ≈ 6.0:1) */
  --primary-hover: #7E2F1C;  /* darker clay — hover/active */
  --accent:        #C8892E;  /* warm amber/gold — small accents, underlines, focus glow */
  --secondary:     #3E5641;  /* deep forest — optional secondary buttons / "rooted" motif */
  --border:        #E3D5C6;  /* warm hairline borders */
  --focus:         #1F1A17;  /* near-black focus outline base; paired with accent glow */
CONTRAST RULES (must hold — Accessibility §7):
- Body text (--text) on --bg and on --surface: passes AAA.
- --text-muted only for non-essential secondary text on --bg/--surface (AA, ≥4.5:1).
- Button label is #FFFFFF on --primary (≥4.5:1). Never put --accent text on --bg for body
  copy (amber on ivory fails contrast) — use --accent only for borders, underlines, icons,
  and decorative fills, never for readable body text.
- Do not place light text on --bg-alt for body copy; --text on --bg-alt still passes.

--- 3.3 SPACING, LAYOUT, GRID ---

SPACING SCALE (8px base; CSS custom properties):
  --space-1: 0.25rem;  --space-2: 0.5rem;  --space-3: 0.75rem; --space-4: 1rem;
  --space-5: 1.5rem;   --space-6: 2rem;    --space-7: 3rem;    --space-8: 4rem;
  --space-9: 6rem;     --space-10: 8rem;
LAYOUT:
  --content-max: 1200px;     /* outer container max width */
  --measure: 65ch;           /* max width for running text blocks */
  --radius: 14px;            /* cards, buttons (soft, premium, not pill, not sharp) */
  --radius-sm: 8px;
  --shadow-sm: 0 1px 2px rgba(36,28,23,.06), 0 2px 8px rgba(36,28,23,.05);
  --shadow-md: 0 6px 24px rgba(36,28,23,.10);
- Centered container: width 100%, max-width var(--content-max), inline padding
  clamp(1rem, 5vw, 2rem), margin-inline auto.
- Section vertical rhythm: padding-block clamp(var(--space-7), 8vw, var(--space-9)).
- Generous whitespace is a feature, not a gap — do not crowd. Alternate --bg and --bg-alt
  section bands to create calm rhythm.
GRID:
- Service lanes / card groups: CSS Grid, repeat(auto-fit, minmax(260px, 1fr)), gap
  var(--space-5). Collapses to one column on phones automatically.
- Portfolio gallery: repeat(auto-fill, minmax(220px, 1fr)), gap var(--space-4).
- Two-column "text + image" rows: single column on mobile; at --bp-md become two columns
  (1fr 1fr) with gap var(--space-7).

--- 3.4 BREAKPOINTS (mobile-first; min-width media queries) ---
  --bp-sm: 480px;    /* large phones */
  --bp-md: 768px;    /* tablets — two-column rows begin */
  --bp-lg: 1024px;   /* desktop nav appears inline; hamburger hides */
  --bp-xl: 1280px;
Design and write CSS phone-first; layer enhancements upward at each min-width.

--- 3.5 MOTION (restrained, tasteful, never gimmicky) ---
- Transitions: 150–220ms ease on color, background, transform, box-shadow for interactive
  elements (buttons, cards, links).
- Hover: buttons darken to --primary-hover and lift translateY(-1px); cards lift
  translateY(-2px) + --shadow-md.
- Scroll reveal: elements with class .reveal start opacity 0 / translateY(12px) and animate
  to visible via IntersectionObserver (see §6). Subtle only.
- prefers-reduced-motion: reduce → disable all transforms/reveal animations; show everything
  immediately. This is mandatory.

--- 3.6 COMPONENTS (define once in CSS, reuse everywhere) ---

BUTTON (.btn):
- Base: Inter 600, --fs-base, padding .8em 1.5em, border-radius var(--radius), border 0,
  cursor pointer, transition per §3.5, min touch target 44px height.
- .btn--primary: background var(--primary); color #fff; hover background var(--primary-hover).
- .btn--secondary: background transparent; color var(--text); border 1.5px solid var(--text);
  hover background var(--text), color var(--bg).
- .btn--ghost (text link CTA, e.g., "Explore loc services →"): no background, color
  var(--primary), font-weight 600, with the literal arrow from copy; hover underline.
- Focus-visible: 3px solid outline using --focus + 2px offset + soft --accent glow.

CARD (.card): background var(--surface); border 1px solid var(--border); border-radius
var(--radius); padding var(--space-5); box-shadow --shadow-sm; hover lift per §3.5. Used for
service lanes, pricing groups, review items, gallery captions.

SECTION (.section): the vertical-rhythm wrapper from §3.3. Variant .section--alt sets
background var(--bg-alt). Each section contains a .container.

NAV / HEADER (.site-header): sticky top, background var(--bg) with bottom 1px var(--border),
subtle shadow on scroll. Left: wordmark "Rooted Hair Studio" (text logo in Fraunces 600,
links to index.html). Right: primary nav links + a persistent .btn--primary "Book Now"
(links to book.html). Below --bp-lg: nav collapses behind a hamburger button
(.nav-toggle, aria-controls + aria-expanded) that opens a full-width panel; the "Book Now"
button stays visible at all widths. See §4 for exact nav item list and §6 for behavior.

FOOTER (.site-footer): background var(--text) (espresso) with --bg-colored text; three zones:
(1) NAP block (the canonical NAP, identical to Contact page and JSON-LD), (2) nav link
columns (services, studio, book), (3) social links [OWNER TO SUPPLY] + copyright line
"© [current year] Rooted Hair Studio." Footer is identical on every page.

--- 3.7 IMAGERY (Cultural Lens — non-negotiable) ---
- EVERY photo is a real photo of type 3–4 hair and/or locs styled at Rooted. Never stock,
  never straight-hair, never a competitor's image (Copy p.45–47, p.438–440; Strategy §6 #1).
- All gallery and hero images are [OWNER TO SUPPLY] (Copy Missing Facts 23). Until supplied,
  render a labeled placeholder box (see §4 placeholder pattern) — NEVER a stand-in stock
  photo.
- Image formats: prefer .webp with a .jpg fallback; width/height attributes set to prevent
  layout shift; loading="lazy" on all below-the-fold images; the hero image loads eagerly.
- Alt text: from the copy's alt-text direction, in HER vocabulary, per actual photo
  ([OWNER TO SUPPLY] per image — Copy p.442–445). No generic "woman getting hair done."


================================================================
4. PAGE-BY-PAGE BUILD (structure + copy mapping + per-page SEO & schema)
================================================================

GLOBAL RULES FOR EVERY PAGE:
- Place ALL copy verbatim from copy-output.md. Do not paraphrase, soften, or rewrite. Keep
  every [OWNER TO SUPPLY: ...] string visible on the page exactly as written.
- Exactly one <h1>. Section headings are <h2>; sub-items within a section are <h3>. Maintain
  hierarchy with no skipped levels.
- Semantic landmarks on every page: <header> (shared), <nav> (in header + footer), <main>,
  <section> per content block, <article> for self-contained items (each service, each review,
  each gallery item), <footer> (shared).
- Per-page <title> and <meta name="description"> exactly as specified below. Owner-specific
  facts inside meta are placeholders.
- Each page carries the global HairSalon JSON-LD (§5.3) PLUS any page-specific schema noted.

PLACEHOLDER RENDER PATTERN (use everywhere a placeholder appears):
- Inline text placeholders (price, phone, etc.): render the literal bracketed string inside a
  <span class="placeholder" data-owner-supply> e.g.
  <span class="placeholder" data-owner-supply>[OWNER TO SUPPLY: actual starting price]</span>
  Style .placeholder distinctly (dotted underline, muted color, small "to be confirmed"
  affordance) so it is visibly provisional and easy to find — but it MUST still render as the
  bracketed text, never blank, never a fake value.
- Image placeholders: a <figure class="img-placeholder"> with an aria-label describing what
  goes there (e.g., "Owner to supply: real portfolio photo of starter locs on type 4 hair,
  Durham") and visible caption text of the bracketed instruction. No stock image.
- Embed placeholders (booking, map): a bordered box containing the bracketed instruction and,
  where the copy provides one, the supporting microcopy around it.

--- SHARED HEADER (identical, hard-coded on every page) ---
Markup outline:
<header class="site-header">
  <div class="container header-inner">
    <a class="wordmark" href="index.html">Rooted Hair Studio</a>
    <button class="nav-toggle" aria-controls="primary-nav" aria-expanded="false">
      <span class="sr-only">Menu</span> <!-- hamburger icon (inline SVG) --></button>
    <nav id="primary-nav" class="primary-nav" aria-label="Primary">
      <ul>
        <li><a href="locs.html">Locs</a></li>
        <li><a href="silk-press.html">Silk Press</a></li>
        <li><a href="protective-styles.html">Protective Styles</a></li>
        <li><a href="scalp-root-health.html">Scalp &amp; Root Health</a></li>
        <li><a href="portfolio.html">Portfolio</a></li>
        <li><a href="reviews.html">Reviews</a></li>
        <li><a href="pricing.html">Pricing</a></li>
        <li><a href="about.html">About</a></li>
        <li><a href="contact.html">Visit</a></li>
      </ul>
    </nav>
    <a class="btn btn--primary header-cta" href="book.html">Book Now</a>
  </div>
</header>
- Mark the current page's nav link with aria-current="page".
- Nav label wording uses her vocabulary; "Visit" maps to contact.html (matches Copy p.594
  "Come see us").

--- SHARED FOOTER (identical, hard-coded on every page) ---
Contains the CANONICAL NAP BLOCK exactly as in Copy p.600–615 (must be byte-identical to the
Contact page NAP and to the JSON-LD — NAP consistency, Research §5 #3, Strategy §6):
  Rooted Hair Studio
  [OWNER TO SUPPLY: full street address, Durham, NC + ZIP]
  [OWNER TO SUPPLY: studio phone number]
  [OWNER TO SUPPLY: contact email]
  [OWNER TO SUPPLY: days and hours of operation]
Plus: footer nav columns (Services → the four service pages; Studio → About, Reviews,
Portfolio, Pricing; Visit/Book → Contact, Book Now); social links
[OWNER TO SUPPLY: Instagram handle / social links] (Copy p.487); copyright line.
The footer's business name, address, phone format, and hours text MUST match the JSON-LD
strings character-for-character.


================================================================
PAGE 1 — index.html  (HOMEPAGE)  · priority HIGHEST
================================================================
Personas: D (lead) + A (co-lead); B/C secondary (Strategy §7 #1, §8 Homepage).
Copy source: copy-output.md p.21–143. Place all copy verbatim.

<title>Rooted Hair Studio | Natural Hair &amp; Loc Specialist in Durham, NC</title>
<meta name="description" content="One trusted Durham studio for your whole natural-hair
journey — locs, silk press, protective styles, and scalp &amp; root care for type 3–4 hair
and locs. Book online.">
(Keywords from Research §5: "natural hair salon Durham," "locs," "silk press," "type 3–4
hair." No owner facts in this meta, so no placeholder needed here.)

SECTION MAP (in order):
1. <section> HERO (class hero)
   - <h1> = Hero headline: "New to Durham? Your hair already has a home here." (Copy p.32)
   - <p class="lead"> = Hero subhead (Copy p.35–37, verbatim)
   - <a class="btn btn--primary"> = "Book Now" → book.html (Copy p.40)
   - <p class="hero-microcopy"> = "Real photos of hair like yours below. Booking takes a
     minute." (Copy p.43)
   - Hero image: <figure class="img-placeholder"> per §3.7 / Copy p.45–47
     ([OWNER TO SUPPLY: hero portfolio image of type 3–4 hair and/or locs]). Loads eagerly.
2. <section> SPECIALIZATION STATEMENT (the "for us" proof)
   - <h2> "We do one thing, and we do all of it well." (Copy p.52)
   - <p> body (Copy p.54–58, verbatim)
3. <section class="section--alt"> FOUR SERVICE LANES
   - <h2> "Your hair, all of it, in expert hands." (Copy p.63)
   - Grid of 4 <article class="card"> (auto-fit grid §3.3), in order Locs, Silk Press,
     Protective Styles, Scalp & Root Health (Copy p.65–87):
       * <h3> lane heading; <p> lane body; <a class="btn--ghost"> lane link text WITH the
         literal "→", linking to the matching service page:
         Locs → locs.html; Silk Press → silk-press.html; Protective Styles →
         protective-styles.html; Scalp & Root Health → scalp-root-health.html.
4. <section> CO-LEAD: THE LOC JOURNEY (Persona A)
   - <h2> "On a loc journey? You're in the right chair." (Copy p.92)
   - <p> body (Copy p.94–98, verbatim)
   - <a class="btn btn--primary"> "Book a loc service" → book.html (Copy p.101)
5. <section class="section--alt"> REASSURANCE STRIP (3 items, Copy p.105–112)
   - Three <article>/<li> items, verbatim:
     Item 1 "Low-tension always. We protect your edges and scalp — no over-tightening."
     Item 2 "Heat-damage-safe. A silk press that protects your curl pattern, not one that
     costs it." Item 3 "Clear pricing and time. You'll know the cost and the hours before you
     sit down." (Use an <h2 class="sr-only">Why clients trust Rooted</h2> for landmark
     labeling since the copy gives no visible heading here.)
6. <section> TRUST / PROOF GLIMPSE
   - <h2> "See hair like yours." (Copy p.117)
   - <p> body (Copy p.120–121, verbatim)
   - <a class="btn--ghost"> "View the full portfolio →" → portfolio.html (Copy p.122)
   - Reviews block: <h3> "What clients say." (Copy p.125) + placeholder per Copy p.127
     ([OWNER TO SUPPLY: real client reviews to feature here — do not borrow or invent.]) +
     <a class="btn--ghost"> "Read more reviews →" → reviews.html (Copy p.128).
     NO review schema, NO sample testimonials.
7. <section class="section--alt"> FINAL CTA
   - <h2> "Ready when you are." (Copy p.133)
   - <p> body (Copy p.135–136, verbatim)
   - <a class="btn btn--primary"> "Book Now" → book.html (Copy p.138)

Per-page schema: global HairSalon JSON-LD only (§5.3). No FAQ schema (no Q&A copy on this
page). No Review/aggregateRating.
Placeholders on this page: hero image; featured reviews.


================================================================
PAGE 2 — book.html  (BOOK NOW)  · priority HIGHEST
================================================================
Copy source: p.145–180. Primary conversion action (Strategy §6).

<title>Book an Appointment | Rooted Hair Studio, Durham NC</title>
<meta name="description" content="Book your appointment at Rooted Hair Studio in Durham, NC —
locs, silk press, protective styles, and scalp &amp; root care for type 3–4 hair and locs.
Online booking takes about a minute.">

SECTION MAP:
1. <section>
   - <h1> "Book your appointment." (Copy p.149)
   - <p class="lead"> subhead (Copy p.152–154, verbatim)
2. <section> SERVICE SELECTION LABELS (Copy p.156–160) — render as a labeled <ul> the user
   can read before booking (these mirror the booking flow):
     Locs — starter locs, retwist & maintenance, loc extensions
     Silk Press — with optional treatment
     Protective Styles — knotless braids, box braids, twists, faux locs
     Scalp & Root Health — deep conditioning, scalp care (add to any service)
3. <section> BOOKING EMBED — embed placeholder box per §4 pattern (Copy p.162–165):
   [OWNER TO SUPPLY: instant online booking link / embed — booking platform and account
   (e.g., Booksy/StyleSeat/Vagaro/GlossGenius) to be confirmed by owner.]
   Build note: when supplied, this is where the platform's embed <iframe> or "Book" deep-link
   button goes; it must be mobile-first and instant (Strategy §6). Until then, show the
   bracketed instruction in the bordered embed box.
4. Supporting microcopy (Copy p.167–169, verbatim):
   "First time at Rooted? Tell us a little about your hair when you book, so your stylist is
   ready for you."
5. Deposit / policy microcopy (Copy p.171–172):
   [OWNER TO SUPPLY: deposit amount, cancellation and late policy, if any.]
6. "Prefer to ask first?" microcopy (Copy p.174–175):
   "Have a question before you book? [OWNER TO SUPPLY: contact phone and/or email.]"

Per-page schema: global HairSalon JSON-LD only. (A ReserveAction could be added once the real
booking URL exists — Open Items; do not add a fake URL now.)
Placeholders: booking embed/platform; deposit & cancellation policy; contact phone/email.


================================================================
PAGE 3 — locs.html  (LOCS)  · priority HIGH (recurring revenue)
================================================================
Copy source: p.182–248. Persona A.

<title>Loc Retwist, Starter Locs &amp; Extensions in Durham, NC | Rooted Hair Studio</title>
<meta name="description" content="Low-tension loc services in Durham, NC — starter locs,
retwist &amp; maintenance, and loc extensions, rooted in scalp health. Pricing
[OWNER TO SUPPLY]. Book your loc service online.">
(Keywords: "loc retwist Durham," "starter locs Durham," Research §5 #1. The price reference is
a placeholder because Rooted's prices are owner-supplied.)

SECTION MAP:
1. <section> HERO/INTRO
   - <h1> "Locs, done right — and kept healthy." (Copy p.186)
   - <p class="lead"> subhead (Copy p.189–192, verbatim)
2. <section> LOC HEALTH POINT OF VIEW
   - <h2> "Your edges are not the price of a clean retwist." (Copy p.197)
   - <p> body (Copy p.199–202, verbatim)
3. <section> SERVICES — three <article class="card"> (Copy p.206–224):
   - Starter Locs: <h3> + body + price/time line carrying placeholders verbatim
     ("Starter locs from [OWNER TO SUPPLY: actual starting price] · approx.
     [OWNER TO SUPPLY: time estimate].")
   - Retwist & Maintenance: <h3> + body + "Retwist from [OWNER TO SUPPLY: actual retwist
     price] · approx. [OWNER TO SUPPLY: time estimate]. Recommended every [OWNER TO SUPPLY:
     recommended interval]."
   - Loc Extensions: <h3> + body + "Loc extensions from [OWNER TO SUPPLY: actual starting
     price] · approx. [OWNER TO SUPPLY: time estimate]."
4. <section class="section--alt"> RELIABILITY PROMISE
   - <h2> "On time, every time you come back." (Copy p.229)
   - <p> body (Copy p.231–234, verbatim)
5. <section> PORTFOLIO + CTA
   - <h2> "See our loc work." (Copy p.238) + <p> body (p.239) + <a class="btn--ghost">
     "View loc portfolio →" → portfolio.html#locs (filter anchor, see §6)
   - <a class="btn btn--primary"> "Book a loc service" → book.html (Copy p.243)

Per-page schema: global HairSalon JSON-LD + Service schema for the loc services (§5.4). All
price fields in Service schema OMITTED (no offers/price) until owner supplies — do NOT emit a
fake price. No FAQPage (no Q&A copy).
Placeholders: starter loc price+time; retwist price+time+interval; extensions price+time; loc
portfolio photos.


================================================================
PAGE 4 — silk-press.html  (SILK PRESS)  · priority HIGH
================================================================
Copy source: p.250–301. Persona B.

<title>Heat-Damage-Safe Silk Press in Durham, NC | Rooted Hair Studio</title>
<meta name="description" content="A heat-damage-safe, curl-protecting silk press in Durham,
NC for type 3–4 and 4c hair — paired with the right treatment so your curl pattern comes back.
Book a silk press online.">
(Keywords: "silk press Durham NC," "heat damage free silk press," "type 4 hair stylist
Durham," Research §5 #1–2.)

SECTION MAP:
1. <section> HERO/INTRO
   - <h1> "A silk press that gives your curls back." (Copy p.254)
   - <p class="lead"> subhead (Copy p.257–259, verbatim)
2. <section> THE HEAT-SAFE METHOD
   - <h2> "Sleek today. Still your curl pattern tomorrow." (Copy p.264)
   - <p> body (Copy p.266–270, verbatim)
3. <section class="section--alt"> TREATMENT PAIRING
   - <h2> "Better paired with a treatment." (Copy p.275)
   - <p> body (Copy p.277–280, verbatim)
4. <section> PRICE / TIME (Copy p.282–288) — two <article>/rows carrying placeholders:
   - "Silk Press: From [OWNER TO SUPPLY: actual silk press price] · approx.
     [OWNER TO SUPPLY: time estimate]."
   - "Silk Press + Treatment: From [OWNER TO SUPPLY: actual silk press + treatment price] ·
     approx. [OWNER TO SUPPLY: time estimate]."
5. <section> PORTFOLIO + CTA
   - <h2> "See our silk press work." (Copy p.292) + <p> body (p.293) + <a class="btn--ghost">
     "View silk press portfolio →" → portfolio.html#silk-press
   - <a class="btn btn--primary"> "Book a silk press" → book.html (Copy p.297)

Per-page schema: global HairSalon JSON-LD + Service schema "Silk Press" (price omitted). No
FAQ schema by default (see §5.5 note on optional, owner-confirmed Q&A).
Placeholders: silk press price+time; silk press + treatment price+time; silk press portfolio
photos.


================================================================
PAGE 5 — protective-styles.html  (PROTECTIVE STYLES / BRAIDS)  · priority HIGH
================================================================
Copy source: p.303–364. Persona C.

<title>Knotless Braids, Box Braids &amp; Faux Locs in Durham, NC | Rooted Hair Studio</title>
<meta name="description" content="Low-tension protective styles in Durham, NC — knotless
braids, box braids, twists, and faux locs with clean parts and no pulled edges. Clear pricing
[OWNER TO SUPPLY] and time before you book.">
(Keywords: "knotless braids Durham," "box braids Durham," Research §5 #1.)

SECTION MAP:
1. <section> HERO/INTRO
   - <h1> "Protective styles that protect — including your edges." (Copy p.307)
   - <p class="lead"> subhead (Copy p.310–312, verbatim)
2. <section> LOW-TENSION / EDGE CARE
   - <h2> "No pulled edges. No headache." (Copy p.316)
   - <p> body (Copy p.319–322, verbatim)
3. <section> SERVICES, PRICE & TIME (Copy p.324–341) — four <article class="card">, each
   carrying placeholders verbatim:
   - Knotless Braids: "From [OWNER TO SUPPLY: actual knotless braids price] · approx.
     [OWNER TO SUPPLY: time estimate]. Length/size add-ons: [OWNER TO SUPPLY: any add-on
     pricing]."
   - Box Braids: "From [OWNER TO SUPPLY: actual box braids price] · approx. [OWNER TO SUPPLY:
     time estimate]."
   - Twists: "From [OWNER TO SUPPLY: actual twists price] · approx. [OWNER TO SUPPLY: time
     estimate]."
   - Faux Locs: "From [OWNER TO SUPPLY: actual faux locs price] · approx. [OWNER TO SUPPLY:
     time estimate]."
   - Pricing microcopy (p.339–341): "Final pricing depends on length, size, and fullness —
     [OWNER TO SUPPLY: how add-on/length pricing works]. No surprises at the chair."
4. <section class="section--alt"> PREP MICROCOPY
   - <h2> "Come ready." (Copy p.345)
   - <p> = [OWNER TO SUPPLY: hair-prep instructions and whether hair/extensions are provided
     or client-supplied.] (Copy p.346–347)
5. <section> PORTFOLIO + CTA
   - <h2> "See our protective style work." (Copy p.351) + <p> body (p.352) +
     <a class="btn--ghost"> "View protective styles portfolio →" →
     portfolio.html#protective-styles
   - <a class="btn btn--primary"> "Book a protective style" → book.html (Copy p.356)

Per-page schema: global HairSalon JSON-LD + Service schema for each protective style (price
omitted). No FAQ schema by default.
Placeholders: knotless/box/twists/faux locs prices+times; add-on pricing rule; prep
instructions/extensions policy; protective styles portfolio photos.


================================================================
PAGE 6 — scalp-root-health.html  (SCALP & ROOT HEALTH / TREATMENTS)  · priority MEDIUM-HIGH
================================================================
Copy source: p.366–419. Strategic anchor (Strategy §6 Gap 2).

<title>Scalp Care &amp; Deep Conditioning in Durham, NC | Rooted Hair Studio</title>
<meta name="description" content="Scalp &amp; root health in Durham, NC — deep conditioning
and scalp care that protect your edges and strengthen your roots, the foundation under every
loc, press, and braid. Add a treatment to any service.">
(Keywords: "scalp treatment / deep conditioning Durham," Research §5 #1.)

SECTION MAP:
1. <section> HERO/INTRO
   - <h1> "Healthy roots are where every style starts." (Copy p.370)
   - <p class="lead"> subhead (Copy p.373–376, verbatim)
2. <section> WHY SCALP & ROOT HEALTH
   - <h2> "We lead with what's under the style." (Copy p.381)
   - <p> body (Copy p.383–386, verbatim)
3. <section class="section--alt"> TRACTION ALOPECIA / EDGE HEALTH
   - <h2> "Protecting your edges is part of the work." (Copy p.391)
   - <p> body (Copy p.393–396, verbatim)
4. <section> SERVICES — two <article class="card"> (Copy p.398–406):
   - Deep Conditioning Treatment: <h3> + body + "From [OWNER TO SUPPLY: actual treatment
     price] · approx. [OWNER TO SUPPLY: time estimate]."
   - Scalp Care: <h3> + body = "[OWNER TO SUPPLY: description of specific scalp-care
     service(s) offered.]" + "From [OWNER TO SUPPLY: actual scalp care price] · approx.
     [OWNER TO SUPPLY: time estimate]."
5. <section> CROSS-SELL CTA
   - <h2> "Add it to your next appointment." (Copy p.410)
   - <p> body (Copy p.411–412, verbatim)
   - Two CTAs (Copy p.414–415): <a class="btn btn--primary"> "Book a treatment" → book.html
     · <a class="btn btn--secondary"> "Add to a styling appointment" → book.html

Per-page schema: global HairSalon JSON-LD + Service schema "Deep Conditioning Treatment" and
"Scalp Care" (descriptions/price as placeholders → price omitted from schema; scalp-care
description omitted from schema until supplied).
Placeholders: deep conditioning price+time; scalp-care description+price+time.


================================================================
PAGE 7 — portfolio.html  (PORTFOLIO / GALLERY)  · priority HIGH
================================================================
Copy source: p.421–452. The decisive conversion asset (Strategy §6 #1).

<title>Portfolio — Locs, Silk Press &amp; Protective Styles on Type 3–4 Hair | Rooted Hair
Studio, Durham</title>
<meta name="description" content="Real work on type 3–4 hair and locs in Durham, NC — locs,
silk press, protective styles, and treatments. No stock, no straight-hair fillers.">

SECTION MAP:
1. <section> INTRO
   - <h1> "Hair like yours, done here." (Copy p.425)
   - <p class="lead"> subhead (Copy p.428–430, verbatim)
2. <section> FILTERABLE GALLERY (Copy p.432–445)
   - Filter controls: a group of buttons (role="group", aria-label "Filter portfolio by
     service") — "All," "Locs," "Silk Press," "Protective Styles," "Scalp & Root Health /
     Treatments." Each filter button has data-filter matching the category. (JS behavior §6;
     works without JS by showing all.)
   - Category anchors for cross-page links: give the gallery sections/ids id="locs",
     id="silk-press", id="protective-styles", id="treatments" so deep links from service
     pages (portfolio.html#locs etc.) land correctly.
   - Gallery grid (§3.3): each item is a <figure class="img-placeholder" data-category="...">
     carrying the photo placeholder (Copy p.438–440) and an alt-text placeholder per image
     (Copy p.442–445), e.g. data-category="locs" with caption/aria
     "[OWNER TO SUPPLY: real portfolio photo — e.g., 'low-tension retwist on mature locs,
     Durham']". Provide at least the four categories' placeholder groups so the structure and
     filter are demonstrable before photos arrive. NO stock images.
3. <section> CTA — two buttons (Copy p.447–448): <a class="btn btn--secondary"> "Find your
   service" → index.html#services (or the lanes section) · <a class="btn btn--primary">
   "Book Now" → book.html.

Per-page schema: global HairSalon JSON-LD only. (ImageObject schema may be added per real
photo once supplied, with owner-written captions — Open Items; do not invent captions.)
Placeholders: all portfolio photos per category; per-image alt text.


================================================================
PAGE 8 — reviews.html  (REVIEWS / WHAT CLIENTS SAY)  · priority HIGH
================================================================
Copy source: p.454–492.

<title>Client Reviews | Rooted Hair Studio, Durham NC</title>
<meta name="description" content="Real words from Rooted Hair Studio clients in Durham, NC
about their locs, silk press, and protective styles. [OWNER TO SUPPLY: reviews pending.]">
(Meta carries a placeholder because there are no real reviews yet; do not invent a review
count or rating in meta.)

SECTION MAP:
1. <section> INTRO
   - <h1> "What clients say." (Copy p.458)
   - <p class="lead"> subhead (Copy p.461–462, verbatim)
2. <section> REVIEWS CONTENT (Copy p.464–471)
   - Render the review-structure placeholder, NOT sample testimonials:
     a single bordered block stating [OWNER TO SUPPLY: genuine Rooted client reviews. Do not
     borrow competitor ratings or write sample testimonials...]. Provide the empty review
     template structure (commented in HTML) so real reviews can be dropped in later: each
     real review becomes an <article class="card review"> with client first name/initial,
     service received, and review text — all [OWNER TO SUPPLY]. Until then, only the
     placeholder shows.
3. <section class="section--alt"> LEAVE A REVIEW
   - <h2> "Loved your hair? Tell the next client." (Copy p.476)
   - <p> body (Copy p.478–479, verbatim)
   - CTAs (Copy p.482–484): <a class="btn btn--primary"> "Leave a review on Google" →
     [OWNER TO SUPPLY: Google review link] · plus [OWNER TO SUPPLY: review platform links —
     Google Business Profile, Booksy/StyleSeat, etc.]
   - Share microcopy (Copy p.486–487): "Tag us so we can share your style. [OWNER TO SUPPLY:
     Instagram handle / social links.]"

Per-page schema: global HairSalon JSON-LD ONLY. ABSOLUTELY NO Review or aggregateRating
schema — none exist (Strategy §4 NOTE, §9 A; Copy p.464–466). This is a hard rule.
Placeholders: all reviews; review platform links; Instagram/social.


================================================================
PAGE 9 — about.html  (ABOUT / THE STUDIO)  · priority MEDIUM
================================================================
Copy source: p.494–542.

<title>About — Natural Hair &amp; Loc Studio in Durham, NC | Rooted Hair Studio</title>
<meta name="description" content="Why we're called Rooted: a Durham, NC natural-hair studio
built on healthy scalp and healthy roots, specializing in type 3–4 hair and locs — locs, silk
press, protective styles, and scalp care.">

SECTION MAP:
1. <section> INTRO
   - <h1> "Why we're called Rooted." (Copy p.498)
   - <p class="lead"> subhead (Copy p.501–503, verbatim)
2. <section> THE POINT OF VIEW
   - <h2> "Your hair was never the exception here." (Copy p.508)
   - <p> body (Copy p.510–515, verbatim)
3. <section class="section--alt"> THE SCALP/ROOT PHILOSOPHY
   - <h2> "We start at the root." (Copy p.520)
   - <p> body (Copy p.522–525, verbatim)
4. <section> THE STUDIO / STYLIST
   - <h2> = [OWNER TO SUPPLY: studio founding story / stylist name(s).] (Copy p.530)
   - <p> = [OWNER TO SUPPLY: owner/stylist background, years of experience, training, and any
     credentials...] (Copy p.532–535). Render the bracketed instruction verbatim. Do NOT
     state or imply tenure, experience, a rating, or "years in business" anywhere on this
     page (Strategy §9 A; Copy p.533–535).
   - <a class="btn btn--primary"> "Book Now" → book.html (Copy p.538)

Per-page schema: global HairSalon JSON-LD only. No Person schema until a real stylist
name/credential is supplied (do not invent founder identity).
Placeholders: founding story/stylist name(s); owner background/credentials.


================================================================
PAGE 10 — pricing.html  (PRICING)  · priority HIGH
================================================================
Copy source: p.544–588. ALL figures are owner-supplied. The $150–$330 market range from the
strategy is competitor/market context ONLY and must NOT appear as Rooted's price (Copy
p.548–551, Strategy §9 A).

<title>Pricing — Locs, Silk Press &amp; Braids in Durham, NC | Rooted Hair Studio</title>
<meta name="description" content="Clear pricing at Rooted Hair Studio, Durham NC — what each
service starts at and about how long it takes. Prices [OWNER TO SUPPLY]. No surprises at the
chair.">

SECTION MAP:
1. <section> INTRO
   - <h1> "Clear pricing. No surprises at the chair." (Copy p.553)
   - <p class="lead"> subhead (Copy p.556–558, verbatim)
2. <section> PRICE TABLES (Copy p.560–577) — four grouped lists, each as a <table> or
   definition list within a <article class="card">, every price and time a placeholder
   carried verbatim:
   - Locs: Starter locs / Retwist & maintenance / Loc extensions — each "from
     [OWNER TO SUPPLY: price] · approx. [OWNER TO SUPPLY: time]"
   - Silk Press: Silk press / Silk press + treatment — same placeholder pattern
   - Protective Styles: Knotless braids / Box braids / Twists / Faux locs — same pattern
   - Scalp & Root Health: Deep conditioning treatment / Scalp care — same pattern
3. Pricing microcopy (Copy p.579–581, verbatim):
   "Final price depends on length, size, and fullness. [OWNER TO SUPPLY: explanation of how
   add-on/length pricing works, plus any deposit required at booking.]"
4. <a class="btn btn--primary"> "Book Now" → book.html (Copy p.583)

Per-page schema: global HairSalon JSON-LD only. Do NOT emit Offer/PriceSpecification schema
with placeholder prices (would either be fake or invalid). Add Offers later when real prices
exist (Open Items).
Placeholders: every price and time across all services; add-on/length pricing rules; deposit
policy.


================================================================
PAGE 11 — contact.html  (CONTACT / VISIT)  · priority MEDIUM-HIGH
================================================================
Copy source: p.590–633. NAP must be byte-identical to the footer and JSON-LD.

<title>Visit Us in Durham, NC | Rooted Hair Studio</title>
<meta name="description" content="Visit Rooted Hair Studio in Durham, NC. Address, hours, and
directions [OWNER TO SUPPLY]. Book your appointment for locs, silk press, or protective styles
online.">

SECTION MAP:
1. <section> INTRO
   - <h1> "Come see us." (Copy p.594)
   - <p class="lead"> subhead (Copy p.597–598, verbatim)
2. <section> NAP BLOCK (Copy p.600–615) — the canonical NAP, identical to footer & schema:
   - Studio name: "Rooted Hair Studio"
   - Address: [OWNER TO SUPPLY: full street address, Durham, NC + ZIP]
   - Phone: [OWNER TO SUPPLY: studio phone number] (when supplied, wrap in tel: link)
   - Email: [OWNER TO SUPPLY: contact email] (when supplied, wrap in mailto: link)
   - Hours: [OWNER TO SUPPLY: days and hours of operation]
   Mark up with appropriate microdata-friendly structure but rely on the JSON-LD (§5.3) as
   the primary structured data.
3. <section> MAP — embed placeholder (Copy p.617–619):
   [OWNER TO SUPPLY: Google Business Profile / map embed — confirm NAP matches the listing
   exactly.]
4. Parking / arrival microcopy (Copy p.621–622):
   [OWNER TO SUPPLY: parking or arrival notes, if any.]
5. CTAs (Copy p.624–625): <a class="btn btn--secondary"> "Get directions" →
   [OWNER TO SUPPLY: map/directions link] · <a class="btn btn--primary"> "Book Now" →
   book.html

Per-page schema: global HairSalon JSON-LD (this page is the natural home for the full NAP in
schema). No fake geo/address — all NAP fields are placeholders in the JSON-LD too (§5.3).
Placeholders: full address+ZIP; phone; email; hours; map embed; parking notes; directions
link.


================================================================
5. SEO AND AI SEO LAYER (GLOBAL)
================================================================

--- 5.1 TRADITIONAL SEO (settled) ---
- Semantic HTML throughout: one <h1>/page, correct heading order, <header>/<nav>/<main>/
  <section>/<article>/<footer> landmarks (per §4).
- Unique <title> + <meta name="description"> per page (specified per-page in §4). Owner facts
  inside them are placeholders.
- Canonical tags: <link rel="canonical" href="[OWNER TO SUPPLY: site root URL]/<page>"> on
  each page. Until the domain exists, carry the placeholder for the root; do not invent a
  domain.
- Open Graph + Twitter card tags per page (og:title, og:description, og:type=website,
  og:image=[OWNER TO SUPPLY: share image], og:url=[OWNER TO SUPPLY root]/<page>). og:image
  is a placeholder until a real photo is supplied — no stock.
- Favicon/touch icons: [OWNER TO SUPPLY: logo/favicon assets].
- Image alt text on every image, from the copy's alt direction, in her vocabulary;
  placeholders where photos are owner-supplied (§3.7, Page 7).
- robots.txt + sitemap.xml (5.6, 5.7). NAP identical everywhere (footer = contact = schema).

--- 5.2 AI SEO / ANSWER-ENGINE VISIBILITY ---
Verified against live sources on 2026-06-04 (see ACCURACY CHECK 5.8).
- Unmistakable business entity: the canonical name, one-line description, location, and
  service list from §1 are echoed verbatim in the footer, the About page, and the HairSalon
  JSON-LD — consistent across the whole site so answer engines resolve one clear entity.
- Question-and-answer structuring: see 5.5 — provisional pending owner-confirmed FAQ copy.
- robots.txt deliberately ALLOWS AI crawlers (5.6) because the goal is AI visibility.

--- 5.3 GLOBAL SCHEMA: HairSalon (LocalBusiness) JSON-LD ---
Place this block (as <script type="application/ld+json">) on EVERY page. All owner facts are
placeholders — emit them as the literal bracketed strings is NOT valid JSON, so follow this
rule: for any unknown field, OMIT the property entirely rather than writing a fake value or a
bracket into JSON. Build the object with only the known-true fields + a clear HTML comment
above it listing which properties to add once supplied. Known-true now: @type, name,
description, areaServed, knowsAbout/services, url(placeholder-comment). Address/telephone/
email/openingHours/geo/image/priceRange = ADD WHEN SUPPLIED.

Template (known-true fields only; commented TODOs for the rest):
<!-- TODO when owner supplies: add "address" (streetAddress, postalCode), "telephone",
     "email", "openingHoursSpecification", "geo", "image", "priceRange", "sameAs"
     (social/booking URLs), and set real "url". Do NOT add aggregateRating or review. -->
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "HairSalon",
  "name": "Rooted Hair Studio",
  "description": "A natural-hair studio in Durham, NC specializing in type 3–4 hair and locs — locs, silk press, protective styles, and scalp & root health.",
  "areaServed": { "@type": "City", "name": "Durham", "containedInPlace": { "@type": "State", "name": "North Carolina" } },
  "address": { "@type": "PostalAddress", "addressLocality": "Durham", "addressRegion": "NC", "addressCountry": "US" },
  "knowsAbout": ["locs", "starter locs", "loc retwist", "loc extensions", "silk press", "knotless braids", "box braids", "twists", "faux locs", "deep conditioning", "scalp care", "type 3–4 hair", "4c hair"],
  "hasOfferCatalog": {
    "@type": "OfferCatalog",
    "name": "Services",
    "itemListElement": [
      { "@type": "OfferCatalog", "name": "Locs", "itemListElement": [
        { "@type": "Offer", "itemOffered": { "@type": "Service", "name": "Starter Locs" } },
        { "@type": "Offer", "itemOffered": { "@type": "Service", "name": "Retwist & Maintenance" } },
        { "@type": "Offer", "itemOffered": { "@type": "Service", "name": "Loc Extensions" } } ] },
      { "@type": "Offer", "itemOffered": { "@type": "Service", "name": "Silk Press" } },
      { "@type": "OfferCatalog", "name": "Protective Styles", "itemListElement": [
        { "@type": "Offer", "itemOffered": { "@type": "Service", "name": "Knotless Braids" } },
        { "@type": "Offer", "itemOffered": { "@type": "Service", "name": "Box Braids" } },
        { "@type": "Offer", "itemOffered": { "@type": "Service", "name": "Twists" } },
        { "@type": "Offer", "itemOffered": { "@type": "Service", "name": "Faux Locs" } } ] },
      { "@type": "OfferCatalog", "name": "Scalp & Root Health", "itemListElement": [
        { "@type": "Offer", "itemOffered": { "@type": "Service", "name": "Deep Conditioning Treatment" } },
        { "@type": "Offer", "itemOffered": { "@type": "Service", "name": "Scalp Care" } } ] }
    ]
  }
}
</script>
NOTE: addressLocality/Region/Country are true facts from the input (Durham, NC, US), so they
are included; streetAddress and postalCode are OMITTED until supplied. No price is emitted in
any Offer (no "priceSpecification") until real prices exist — an Offer without a price is
valid; a fake price is not.

--- 5.4 PER-SERVICE Service SCHEMA (service pages) ---
On locs.html, silk-press.html, protective-styles.html, scalp-root-health.html, add a Service
JSON-LD per service, provider = the HairSalon entity, areaServed Durham NC, serviceType in her
vocabulary. OMIT "offers"/price until real prices exist. Example (locs.html, Starter Locs):
{ "@context":"https://schema.org", "@type":"Service", "serviceType":"Starter Locs",
  "provider":{"@type":"HairSalon","name":"Rooted Hair Studio"},
  "areaServed":{"@type":"City","name":"Durham"} }
Repeat per service named on that page. No invented attributes.

--- 5.5 FAQPage SCHEMA — PROVISIONAL, GAP FLAGGED ---
GAP: copy-output.md contains NO explicit FAQ / Q&A block. Per the NO-FABRICATION rule and
Google policy, FAQPage schema is added ONLY where real Q&A copy exists. Therefore:
- By default, NO FAQPage schema is emitted on any page in this build.
- This is flagged as a content gap to fill (AI-SEO requirement, Strategy §6, Research §5 #2
  reassurance intent). The research already lists the questions this audience asks
  (Strategy §4 objections 1–5; Research §5 #2): "Can they really do MY hair?", "Will they
  damage my hair with heat?", "Will they over-tighten my locs / wreck my edges?",
  "What will it cost and how long will it take?".
- RECOMMENDED (do at build only if owner/copy approves): add a short FAQ section to the
  relevant service pages where the QUESTION comes from the list above and the ANSWER is drawn
  WITHOUT adding any new fact — i.e., reusing already-approved copy on that page (e.g., the
  silk-press heat-safe explanation, the locs low-tension explanation). Cost/time answers stay
  as placeholders and must NOT go into schema. If and only if those Q&A pairs are added as
  real on-page copy, emit FAQPage schema for exactly those pairs, and build the accordion
  (§6). Until then: question structure is a flagged gap, not invented content.

--- 5.6 robots.txt (AI crawlers ALLOWED on purpose) ---
Create projects/rooted-hair-studio/site/robots.txt. Goal is AI visibility, so allow both
traditional and AI crawlers; reference the sitemap. Verified token list (2026, see 5.8):
  User-agent: *
  Allow: /

  # AI answer-engine retrieval/search crawlers (allowed for AI visibility)
  User-agent: OAI-SearchBot
  Allow: /
  User-agent: ChatGPT-User
  Allow: /
  User-agent: PerplexityBot
  Allow: /
  User-agent: Perplexity-User
  Allow: /
  User-agent: Claude-SearchBot
  Allow: /
  User-agent: Claude-User
  Allow: /
  User-agent: Google-Extended
  Allow: /

  # AI training crawlers — allowed (owner's goal is visibility, not blocking)
  User-agent: GPTBot
  Allow: /
  User-agent: ClaudeBot
  Allow: /
  User-agent: CCBot
  Allow: /

  Sitemap: [OWNER TO SUPPLY: site root URL]/sitemap.xml
NOTE for build: the Sitemap line needs the real domain; carry the placeholder until the owner
supplies it. The owner may later choose to disallow training bots (GPTBot/ClaudeBot/CCBot/
Google-Extended) while keeping the search bots — that's a values choice (Open Items). Default
here = allow all, matching the stated AI-visibility goal.

--- 5.7 sitemap.xml ---
Create a standard XML sitemap listing all 11 HTML pages, using [OWNER TO SUPPLY: site root
URL] as the base (carry the placeholder until the domain exists; do not invent a domain).
Priority hints: index 1.0; book 0.9; locs/silk-press/protective-styles/portfolio/reviews/
pricing 0.8; scalp-root-health/contact 0.7; about 0.6. lastmod = build date.

--- 5.8 ACCURACY CHECK (AI-SEO best practice, verified live 2026-06-04) ---
SETTLED (safe to bake in):
- Allowing AI crawlers via robots.txt with the user-agent tokens in 5.6 is current and
  correct; OpenAI/Anthropic/Perplexity/Google publish and honor these tokens. The
  training-vs-search split (e.g., Claude-SearchBot independent of ClaudeBot; OAI-SearchBot vs
  GPTBot) is real as of 2026.
- Schema.org JSON-LD (LocalBusiness/HairSalon, Service), one clear entity, consistent NAP,
  semantic HTML, and direct on-page answers to real audience questions remain the durable,
  effective basis for both traditional and answer-engine visibility.
PROVISIONAL (do NOT bake in as fact):
- llms.txt: ~10% site adoption but NO major AI platform (OpenAI, Anthropic, Perplexity,
  Google) commits to reading it as of 2026, and measured crawler hits are negligible. DO NOT
  ship an llms.txt as a load-bearing SEO tactic. It is listed in Open Items as optional only;
  if the owner wants one as a low-cost experiment, it may be added later, clearly labeled
  experimental — but it is not part of this build's settled layer.
- FAQPage schema: provisional per 5.5 (pending real Q&A copy).
- Specific keyword search volumes: not verified (Research §5 erasure-honesty); do not publish
  or imply volume numbers.


================================================================
6. JAVASCRIPT SPEC (js/main.js — vanilla, progressive enhancement)
================================================================
All features degrade gracefully: with JS off, the site is fully usable. No framework, no
dependencies. Wrap everything in a DOMContentLoaded guard; respect prefers-reduced-motion.

1. MOBILE MENU (all pages)
   - Toggle .nav-toggle controls #primary-nav. On click: flip aria-expanded true/false, add/
     remove an .is-open class that reveals the nav panel (CSS handles the slide/fade).
   - Close on: Escape key, clicking a nav link, or resizing up past --bp-lg.
   - Without JS: nav links are plain <a>s; CSS shows the nav inline at ≥--bp-lg so desktop is
     unaffected; below that the links remain reachable (CSS fallback: nav visible/stacked if
     .js class is absent). Add a `js` class to <html> via an inline head script so CSS can
     branch on JS presence.

2. PORTFOLIO FILTER (portfolio.html)
   - Filter buttons with data-filter ("all","locs","silk-press","protective-styles",
     "treatments"). On click: set aria-pressed, show only <figure> items whose data-category
     matches (or all for "all") by toggling a hidden attribute/.is-hidden class.
   - Support deep links: on load, read location.hash (#locs, #silk-press,
     #protective-styles, #treatments) and apply that filter so service-page links land
     pre-filtered.
   - Without JS: all photos show (no filtering) — still a complete gallery.

3. FAQ ACCORDION (optional — only if owner-confirmed FAQ copy is added per 5.5)
   - Each FAQ item: a <button aria-expanded> controlling an answer panel by id. Toggle
     expanded/collapsed. Without JS: answers are visible by default (details/summary is an
     acceptable no-JS alternative — prefer <details>/<summary> so it works with zero JS and
     still satisfies the accordion requirement).

4. IMAGE LIGHTBOX (optional, portfolio.html — add once real photos exist)
   - Clicking a gallery image opens an accessible dialog (focus trap, Escape to close,
     restore focus). Keep it tiny and dependency-free. Without JS: images are normal inline
     images. Skip building until real photos are supplied.

5. SCROLL REVEAL (subtle, §3.5)
   - IntersectionObserver adds .is-visible to .reveal elements as they enter the viewport.
     If prefers-reduced-motion: reduce, skip the observer and show everything immediately.

6. STICKY HEADER SHADOW
   - Add .is-scrolled to .site-header after small scroll for a subtle shadow. Cosmetic only.

7. BOOKING WIDGET (book.html)
   - No custom JS needed for a simple embed; the owner's platform [OWNER TO SUPPLY] provides
     its own <iframe>/script. If a script embed is supplied, load it deferred and only on
     book.html. Until supplied, the placeholder box (no JS) stands in.


================================================================
7. ACCESSIBILITY
================================================================
- Semantic landmarks and correct heading order on every page (§4); one h1/page.
- Color contrast per §3.2 (body text AAA on backgrounds; buttons ≥4.5:1; --accent never used
  for readable body text).
- Visible focus states: strong focus-visible outline on all interactive elements (§3.6
  button focus rule) — never remove outlines without a replacement.
- All controls labeled: nav-toggle has an accessible name ("Menu"); filter buttons have text
  labels; icon-only buttons include <span class="sr-only">; form/booking controls (when
  supplied) are labeled.
- Touch targets ≥44×44px (buttons, nav links, filter chips).
- Images: meaningful alt text in her vocabulary; decorative images alt="".
- prefers-reduced-motion honored across all motion (§3.5, §6).
- Keyboard: mobile menu, filters, accordion (details/summary), and lightbox are fully
  keyboard-operable; logical tab order; Escape closes overlays; focus is managed/restored for
  any dialog.
- Skip link: a "Skip to content" link as the first focusable element, jumping to <main id="main">.
- .sr-only utility class for visually-hidden-but-accessible text.


================================================================
8. BUILD SEQUENCE FOR CLAUDE CODE (run order)
================================================================
1. Scaffold projects/rooted-hair-studio/site/ per the §2 file tree (create folders, .gitkeep
   in assets/images/).
2. Build css/styles.css FIRST: implement the full §3 design system as CSS custom properties
   (type, color, spacing, breakpoints, motion) + all components (button, card, section, nav/
   header, footer, .placeholder, .img-placeholder, .sr-only, .reveal). Mobile-first.
3. Build the shared header (§4 Shared Header) and footer (§4 Shared Footer) markup; this exact
   markup is hard-coded identically into every page in the next step.
4. Build each page in sitemap priority order — index.html → book.html → locs.html →
   silk-press.html → protective-styles.html → portfolio.html → reviews.html → pricing.html →
   scalp-root-health.html → about.html → contact.html — mapping in the REAL copy from
   copy-output.md verbatim (§4), keeping EVERY [OWNER TO SUPPLY] placeholder visible via the
   §4 placeholder render pattern.
5. Add per-page semantic structure, <title>, meta description, canonical/OG tags (§4, §5.1),
   the global HairSalon JSON-LD (§5.3), and per-service Service schema on service pages
   (§5.4). NO Review/aggregateRating anywhere. NO FAQPage unless 5.5's owner-confirmed Q&A
   path is taken.
6. Add robots.txt (§5.6, AI crawlers allowed) and sitemap.xml (§5.7), both carrying the
   [OWNER TO SUPPLY: site root URL] placeholder for the domain.
7. Add js/main.js (§6): mobile menu, portfolio filter (+ hash deep-link), scroll reveal,
   sticky-header shadow; accordion/lightbox/booking only per their conditional notes.
8. VERIFY before calling done:
   - Re-confirm the AI-SEO layer against current best practice (§5.8) — especially that no
     unproven tactic (llms.txt) was shipped as load-bearing and the crawler tokens are still
     current.
   - Validate every page: one h1, correct landmarks, valid JSON-LD (no fake/placeholder
     values inside JSON — unknown props omitted), all placeholders visibly present, no stock
     or straight-hair imagery anywhere, NAP identical across footer/contact/schema, all links
     resolve, keyboard + reduced-motion behavior works, contrast holds.


================================================================
9. OPEN ITEMS (placeholders + provisional tactics to confirm before/at build)
================================================================

A. OWNER-SUPPLIED FACTS (carried as visible [OWNER TO SUPPLY] placeholders; consolidated from
   copy-output.md Missing Facts 1–28). The built site shows these brackets until filled — never
   a guessed value:
   Business identity & contact (NAP — must match Google Business Profile exactly):
     1. Full street address (Durham, NC + ZIP)
     2. Studio phone number
     3. Contact email
     4. Days and hours of operation
     5. Google Business Profile / map embed link
     6. Parking / arrival notes (if any)
     7. Instagram handle / social links
   Booking:
     8. Instant online booking link/embed and platform (Booksy/StyleSeat/Vagaro/GlossGenius)
     9. Deposit amount, cancellation, and late policy
    10. How length/size add-on pricing works (across services)
    11. Hair-prep instructions; whether extensions are provided or client-supplied
   Pricing & time (Rooted's own — the $150–$330 strategy figure is MARKET context only, NOT
   Rooted's price; never publish it as Rooted's pricing):
    12–22. Per-service price + time for: starter locs; retwist & maintenance (+ interval);
           loc extensions; silk press; silk press + treatment; knotless braids; box braids;
           twists; faux locs; deep conditioning treatment; scalp care (+ description).
   Proof & credibility (none in research — never state/imply until confirmed; never borrow
   competitor ratings/reviews):
    23. Real portfolio photos of type 3–4 hair and locs, per service lane (hero + gallery)
    24. Per-image alt text matching each actual photo
    25. Genuine Rooted client reviews (name/initial, service, text)
    26. Review platform links (Google Business Profile, Booksy/StyleSeat, etc.)
    27. Studio founding story / stylist name(s)
    28. Owner/stylist background, experience, training, credentials

B. BUILD-LEVEL PLACEHOLDERS (needed for SEO/schema, not in copy):
    29. Site root URL / domain — for canonical tags, OG url, robots.txt Sitemap line,
        sitemap.xml, and the JSON-LD "url"/"sameAs". Carry placeholder until owner registers
        the domain.
    30. Share/OG image and favicon/logo assets (real, on-brand; no stock).
    31. Brand fonts and brand colors — IF the owner has them, mark and swap for the §3
        defaults, then re-check contrast (§3.2).

C. PROVISIONAL AI-SEO TACTICS (verified 2026-06-04, §5.8 — do NOT ship as settled fact):
    32. llms.txt — optional experiment only; no major AI platform commits to reading it as of
        2026 and crawler hits are negligible. Not in the default build. Add later only if the
        owner wants it, clearly labeled experimental.
    33. FAQPage schema + on-page FAQ — provisional (§5.5). Add ONLY if owner-confirmed Q&A
        copy is created, with answers drawn from already-approved copy (no new facts); price/
        time answers must stay placeholders and out of schema.
    34. AI-crawler policy is set to ALLOW ALL (incl. training bots) to match the stated
        AI-visibility goal. If the owner prefers to allow only answer-engine/search bots and
        block training bots (GPTBot/ClaudeBot/CCBot/Google-Extended), update robots.txt
        accordingly — a values choice to confirm.
    35. Specific keyword search volumes unverified (Research §5) — do not publish/imply volume
        figures; confirm with a keyword tool before any volume-based or paid prioritization.

D. SCHEMA HARD RULES (carried, do not relax at build):
    - NO aggregateRating or Review schema anywhere until real Rooted reviews exist.
    - NO Offer price/PriceSpecification until real Rooted prices exist (price-less Offers are
      fine; fake prices are not).
    - NO Person schema for a stylist until a real name/credential is supplied.
    - Inside JSON-LD, OMIT any unknown property — never write a bracketed placeholder or a
      guessed value into the JSON.

--- END OF BUILD SPEC ---
STOP per task instructions: the build specification is saved. The actual site build is a
separate step (run §8 against this spec). No site is built here.
