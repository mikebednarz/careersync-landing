# Careersync — Project Brief for Claude Code

This document is the source of truth for building **trycareersync.com**, the landing page for Careersync, a productized service that builds branded careers pages for startups. Read this fully before writing code. The goal is a single-page marketing site that can be deployed to Vercel today, plus a foundation that can grow into the production careers-page template later.

---

## 1. Business context (read this to make good judgment calls)

**What Careersync sells:** custom-branded careers pages on the client's own domain (`careers.company.com`) that stay automatically in sync with the client's ATS (Greenhouse, Lever, or Ashby) by reading the ATS's public job board API. Fixed-price productized service, not hourly freelancing.

**Who it's for (two segments, both addressed on the page):**
1. Startups whose only careers presence is a raw ATS-hosted board (e.g. `boards.greenhouse.io/company`) — they have a job *list*, not a careers *site*. Sold on: own domain, brand story, Google Jobs indexing.
2. Companies with a custom careers page that is manually updated and has gone stale (dead postings, missing roles). Sold on: automatic sync — "nobody edits a job listing again."

**Key positioning constraints (do not violate in copy):**
- NEVER claim Greenhouse's hosted board can't be branded — it has theming (Design Studio: colors, fonts, banners). The honest differentiator: it's a themeable job *list* on Greenhouse's domain; it cannot become a careers *site* with story/mission/team on the client's own domain.
- The standard tiers link out to the ATS apply form (this is a feature: zero setup, compliance inherited). Only the top tier embeds applications on-site.
- The sync requires nothing installed in the client's ATS — the page reads the public job board feed. Launch requires exactly one DNS record (CNAME) from the client.
- Do not invent ROI statistics or conversion claims. No fake testimonials, no fake client logos. There are no clients yet.

**The founder:** Mike — former technical recruiter turned frontend engineer (React), based in Raleigh, NC. This dual background is the credibility story; the founder section leans on it.

**Voice:** plain, confident, honest, slightly wry. Short sentences. No corporate filler ("leverage", "solutions", "empower"). Reads like a competent person explaining a useful thing.

---

## 2. Tech requirements

- **Stack:** static HTML/CSS (single `index.html`), no framework, no build step. Vanilla JS only if needed (it currently isn't — FAQ uses native `<details>`). This is deliberate: fastest possible deploy, trivial to edit.
- **Fonts** via Google Fonts: `Bricolage Grotesque` (800/600/500) for display/headings, `Public Sans` (400/500/600) for body, `IBM Plex Mono` (400/500) for labels/chips.
- **Responsive** down to 360px. All multi-column grids collapse to one column below 840px. Nav links (except CTA button) hide on mobile.
- **Accessibility:** semantic landmarks, visible focus states, `prefers-reduced-motion` respected (the one animation must disable), decorative visuals `aria-hidden`.
- **SEO:** proper `<title>` and meta description (provided below), single `<h1>`.
- **Performance:** no images required in v1 (the founder photo is the only asset, placeholder until provided). Lighthouse 95+ across the board should be easy — keep it that way.

---

## 3. Design system

**Palette (CSS custom properties on `:root`):**

| Token | Hex | Use |
|---|---|---|
| `--ink` | `#10202B` | text, dark sections, primary buttons |
| `--bg` | `#F6F7F4` | page background (warm fog) |
| `--card` | `#FFFFFF` | cards, panels |
| `--sync` | `#0E9F6E` | signal green — status dots, primary CTA, accents. This is the brand color; it means "live/synced" |
| `--sync-dark` | `#0B7A55` | green hover states, links, eyebrow text |
| `--slate` | `#5B6B72` | secondary text |
| `--line` | `#DDE3DF` | borders |
| `--amber` | `#E8A33D` | reserved; use at most once, sparingly, or not at all |

**Type scale:** h1 clamp(34px→52px) weight 800, letter-spacing -0.015em; h2 clamp(26px→36px) weight 800; body 16.5px/1.6; eyebrows are IBM Plex Mono 12px uppercase letter-spaced 0.14em in `--sync-dark`.

**Signature element — the sync chip.** A pill-shaped status chip: mono text `SYNCED WITH YOUR ATS · JUST NOW` with a 7px green dot that pulses (a soft expanding box-shadow ring, 2.4s ease-in-out infinite; animation removed under `prefers-reduced-motion`). This chip sits at the top of the hero and its visual language (mono labels + green "live" accents) recurs through the page. It is the one memorable device; keep everything else quiet. Do not add other animations.

**General feel:** light, airy, lots of whitespace, 10–16px border radii, hairline borders, one soft shadow on the hero visual only. Not a dark-mode site; the dark `--ink` sections provide contrast rhythm.

---

## 4. Page structure & copy (build exactly this, in this order)

Max content width 1020px, 24px side padding.

### 4.1 Nav
Logo left: the wordmark `careersync.` — lowercase Bricolage Grotesque 800, with the trailing period in `--sync` green. Links right: How it works, Demo, Pricing, FAQ (anchor links), then a green button **Book an intro call** → `TODO_CALENDLY_URL`.

### 4.2 Hero (2-col: 1.1fr text / 0.9fr visual)
- Sync chip (signature element, described above)
- **H1:** `A careers page that looks like your brand and updates itself.` — with "updates itself." in `--sync-dark` (via `<em>` styled non-italic)
- **Lede:** `Careersync builds a branded careers site on your own domain that stays automatically in sync with Greenhouse, Lever, or Ashby. Open a role in your ATS — it's live on your site in minutes. Close it — it's gone. Nobody at your company ever edits a job listing again.`
- CTAs: green **Book an intro call** (`TODO_CALENDLY_URL`) + ghost **See a live demo** (anchor to #demo)
- Subnote (13.5px slate): `Flat price · Launched in about a week · One DNS record to go live`
- **Hero visual** (aria-hidden card): two stacked mini-panels connected by a mono flow line `⇅ synced automatically`.
  - Panel 1 labeled `YOUR ATS · GREENHOUSE`, three rows: "Senior Frontend Engineer / OPEN", "Product Designer / JUST OPENED" (green, bold), "Sales Lead / FILLED" (struck through, faded).
  - Panel 2 labeled `CAREERS.YOURCOMPANY.COM` on a faint green-tinted gradient: only the two live roles, each with `Apply →`. The filled role is absent — the visual demonstrates the sync.

### 4.3 Problem section (full-width dark band, `--ink` background)
- Eyebrow `The problem` (green) · **H2:** `Your job list isn't a careers page.`
- Sub: `Most growing companies send candidates to one of two places: a raw ATS board on someone else's domain, or a hand-built page that quietly went stale months ago. Both cost you candidates before they ever apply.`
- Two comparison boxes side by side:
  - **TODAY** (dark blue box): `boards.greenhouse.io/yourcompany — a list of links on a domain you don't own` / `No story: no mission, team, or reason to join` / `Or worse — a static page listing a role you filled in March` / `Invisible to Google's job search results`
  - **WITH CAREERSYNC** (dark green box): `careers.yourcompany.com — your domain, your brand` / `Mission, team, and culture around every listing` / `Always current — synced from your ATS automatically` / `Structured data that puts roles in Google job search`

### 4.4 How it works (id="how", 3 step cards)
Eyebrow `How it works` · **H2:** `Live in about a week. One DNS record.`
- **STEP 01 — Kickoff:** `One short intake: your ATS board link, brand assets, and the story you want to tell candidates. Thin on content? We'll draft it from what you have.`
- **STEP 02 — Build & review:** `We design and build your page, wired live to your ATS feed, and send a preview link. Two revision rounds included.`
- **STEP 03 — Launch:** `Your team adds one DNS record — careers.yourcompany.com is live with SSL, Google job indexing, and analytics. Nothing to install in your ATS.`

### 4.5 Demo band (id="demo", white card, 2-col)
Eyebrow `See it real` · **H2:** `Tour a live Careersync build.`
Copy: `Tidewater Robotics is our demo client — browse the page, filter roles, open a listing. Everything you see is rendered live from an ATS-shaped feed, exactly how your page would work.`
Button: **Open the demo →** → `TODO_DEMO_URL` (the deployed Tidewater Robotics demo).

### 4.6 Pricing (id="pricing", 3 tier cards)
Eyebrow `Pricing` · **H2:** `Flat price. No hourly meter.`
Sub: `Every tier includes the automatic ATS sync, your own domain, mobile-ready design, and Google Jobs structured data.`

| | Job Board — $1,500 | **Careers Site — $3,000** (featured, "MOST POPULAR" flag, green border) | Careers Site + Apply — $5,000 |
|---|---|---|---|
| Includes | Branded job board on your domain · Search + department & location filters · Automatic ATS sync · Google Jobs indexing | Everything in Job Board · Mission, team & benefits sections · Custom design matched to your brand · Photo & culture content support · Analytics dashboard | Everything in Careers Site · Candidates apply without leaving your site · Application forms styled to your brand · ATS custom questions supported · Priority build queue |
| Excludes (shown greyed with —) | Story & culture sections · On-site applications | On-site applications | — |
| CTA | ghost "Start here" | green "Book an intro call" | ghost "Talk to us" |

All prices labeled `one-time`. Below the grid: `Hosting, monitoring, and small content edits: **$75/mo** after launch. Cancel anytime — the site is yours.`

### 4.7 Founder section
Eyebrow `Who's behind this` · **H2:** `Built by a recruiter who became an engineer.`
Card with 120px photo placeholder (`TODO_PHOTO`) + text:
`**Hi, I'm Mike.** I spent years as a technical recruiter before becoming a frontend engineer — which means I've sent thousands of candidates to careers pages and watched exactly where companies lose them. Careersync is the page I always wished my clients had: branded enough to tell their story, wired to the ATS so it's never out of date, and priced so a growing team can just say yes.`
Then: `Based in Raleigh, NC · LinkedIn` (`TODO_LINKEDIN_URL`).

### 4.8 FAQ (id="faq", native `<details>` accordions)
Eyebrow `Questions` · **H2:** `The things people ask.`

1. **Doesn't Greenhouse already give us a job board?** — `It does — a themeable job list, hosted on Greenhouse's domain. What it can't become is a careers site: your domain, your story, your team and mission around the listings, with Google job indexing accruing to your own website. Careersync is the page around the list — and it stays exactly as current as the board it reads from.`
2. **What does our team have to set up or maintain?** — `Almost nothing. Going live takes one DNS record (we send copy-paste instructions). There's nothing to install or configure inside your ATS, and after launch the page updates itself — open or close roles in your ATS like you always do.`
3. **Which ATS platforms do you support?** — `Greenhouse, Lever, and Ashby today, with more on request. If you're on something else — or tracking roles in a spreadsheet — book a call and we'll figure out the right setup.`
4. **How do candidates apply?** — `On the Job Board and Careers Site tiers, the apply button takes candidates to your ATS's application form, so applications land in your pipeline exactly as they do today. The Careers Site + Apply tier embeds the full application on your site, styled to your brand, so candidates never leave.`
5. **How long does it take?** — `About a week from kickoff to launch for most builds — the main variable is how quickly we get your brand assets and content.`
6. **Who owns the site?** — `You do. If you ever cancel the maintenance retainer, we hand over the code and hosting cleanly.`

### 4.9 Final CTA (dark rounded band, centered)
**H2:** `Your next great hire is about to look you up.`
`Fifteen-minute intro call: we'll look at your current careers presence together and tell you honestly whether this is worth doing.`
Green button **Book an intro call** → `TODO_CALENDLY_URL`.

### 4.10 Footer
Left: `© 2026 Careersync · Raleigh, NC` · Right: `hello@trycareersync.com` mailto link (`TODO_EMAIL` — confirm address exists before launch).

---

## 5. Metadata

- `<title>`: `Careersync — Branded careers pages that stay in sync with your ATS`
- Meta description: `Careersync builds branded careers pages on your own domain that update automatically from Greenhouse, Lever, or Ashby. Launched in about a week, flat price.`

---

## 6. TODO placeholders (grep for `TODO_`)

| Placeholder | Replace with |
|---|---|
| `TODO_CALENDLY_URL` | Mike's Calendly booking link (appears 3×: nav, featured tier, final CTA — plus hero) |
| `TODO_DEMO_URL` | Deployed Tidewater Robotics demo URL |
| `TODO_PHOTO` | Founder headshot (~240×240, can be square-cropped) |
| `TODO_LINKEDIN_URL` | Mike's LinkedIn profile |
| `TODO_EMAIL` | Confirmed hello@ address |

Ship with placeholders as `#` hrefs if needed — do not block deploy on them.

---

## 7. Deployment

1. Repo: `careersync-landing`, single `index.html` at root (plus this brief as `BRIEF.md`).
2. Deploy on Vercel (static, zero config). Preview URL first; confirm mobile layout at 375px and the reduced-motion behavior.
3. Point `trycareersync.com` (and `www`) at the Vercel project; Vercel provisions SSL.
4. Post-launch checks: Lighthouse ≥95, all anchor links land correctly, FAQ accordions work without JS errors (there is no JS), Calendly link opens once wired.

## 8. Later (do not build now, but don't paint into a corner)

- The production careers-page **template** (separate repo): Next.js, ATS adapter interface (Greenhouse first), ISR ~10min revalidation, per-client theme config, JSON-LD JobPosting schema. The landing page's design tokens are unrelated to client builds — every client gets their own brand.
- A `/demo` route or subdomain hosting the Tidewater demo.
- Simple analytics (Plausible) on the landing page.
