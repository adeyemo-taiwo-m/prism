# PRISM — Complete Frontend UI/UX Specification
### For AI Agent: Build Instructions — Every Page, Every Element, Every Pixel

---

## TABLE OF CONTENTS

1. [Product Identity & Design System](#1-product-identity--design-system)
2. [Brand Logic: Why "Prism"](#2-brand-logic-why-prism)
3. [Logo & Visual Identity](#3-logo--visual-identity)
4. [Loader / Splash Screen](#4-loader--splash-screen)
5. [Global Design Tokens](#5-global-design-tokens)
6. [Global Layout & Navigation Shell](#6-global-layout--navigation-shell)
7. [Page: Landing Page (`/`)](#7-page-landing-page-)
8. [Page: Auth — Login (`/login`)](#8-page-auth--login-login)
9. [Page: Auth — Register (`/register`)](#9-page-auth--register-register)
10. [Page: Dashboard — Signal Feed (`/dashboard`)](#10-page-dashboard--signal-feed-dashboard)
11. [Page: Market Detail (`/market/:id`)](#11-page-market-detail-marketid)
12. [Page: Performance Tracker (`/performance`)](#12-page-performance-tracker-performance)
13. [Page: About / Methodology (`/methodology`)](#13-page-about--methodology-methodology)
14. [ADMIN: Admin Layout Shell](#14-admin-admin-layout-shell)
15. [ADMIN: Admin Dashboard (`/admin`)](#15-admin-admin-dashboard-admin)
16. [ADMIN: Admin Markets (`/admin/markets`)](#16-admin-admin-markets-adminmarkets)
17. [ADMIN: Admin Signals (`/admin/signals`)](#17-admin-admin-signals-adminsignals)
18. [ADMIN: Admin System Health (`/admin/system`)](#18-admin-admin-system-health-adminsystem)
19. [Component Library Reference](#19-component-library-reference)
20. [Animation & GSAP Directives](#20-animation--gsap-directives)
21. [Routing & Navigation Flow Map](#21-routing--navigation-flow-map)
22. [Cookie-Based Auth Architecture](#22-cookie-based-auth-architecture)
23. [Responsive Breakpoint Rules](#23-responsive-breakpoint-rules)
24. [Empty, Loading & Error States](#24-empty-loading--error-states)

---

## 1. Product Identity & Design System

### What Prism Is
Prism is a **real-time signal intelligence engine** for prediction markets. It ingests raw market data from Bayse prediction markets, runs a proprietary four-factor Signal Strength Score algorithm grounded in quantitative finance microstructure theory, and uses Google Gemini AI to produce plain-language interpretations. It tells traders whether a price movement is an **INFORMED MOVE**, **UNCERTAIN**, or **NOISE** — in real time, with a number.

### The Core UI Philosophy
The UI must feel like a **Bloomberg Terminal that was redesigned by a modern product team**. This means:
- Information density is high, but hierarchy is crystal clear.
- Color is functional, not decorative — it signals state (green = informed, amber = uncertain, red = noise).
- Typography is the primary design element.
- Whitespace is used strategically, not generously.
- Animations are purposeful: they communicate data updates, not aesthetics.
- Nothing should feel "vibe coded." Every element earns its place.

### What This Is NOT
- Not a crypto exchange (no rainbow colors, no 3D coins).
- Not a consumer fintech app (no rounded blobs, no pastel gradients).
- Not a generic SaaS dashboard (no flat blue + white + sidebar with icons only).
- Not AI-generic (no purple gradients, no "sparkle" iconography everywhere).

---

## 2. Brand Logic: Why "Prism"

A prism takes **white light** — which appears uniform and undifferentiated — and **splits it into its true components**: distinct wavelengths, each visible and interpretable.

In markets:
- **White light = raw market data**: price moves, order flow, liquidity numbers, volume — messy, confusing, easily misleading.
- **The prism = Prism's scoring engine**: it refracts that chaos into three distinct signal bands — Informed, Uncertain, Noise.
- **The refracted spectrum = signal classification**: clear, interpretable, actionable.

This metaphor must live in the product's visual DNA — not as a gimmick, but as a coherent thread:
- The logo is a prism refracting light.
- The loader animates a beam splitting into three colors.
- The three signal classifications map to the three visible bands.
- The landing page hero uses a light-refraction visual metaphor.

---

## 3. Logo & Visual Identity

### Logo Concept
**Shape**: An isometric prism (triangular prism, three-dimensional, rendered as a flat geometric icon). The prism is rendered in deep navy with a thin, precise outline.

**The Refraction**: From the right face of the prism, three thin rays of light emerge and diverge — the top ray is `#16A34A` (green, INFORMED), the middle is `#F59E0B` (amber, UNCERTAIN), the bottom is `#DC2626` (red, NOISE). These rays are 2px wide, straight lines with a slight gradient fade at the tip.

**Wordmark**: "PRISM" in `JetBrains Mono` or `IBM Plex Mono` (monospaced), uppercase, letter-spacing `0.2em`, weight 700. The wordmark sits to the right of the icon. Color is `#E2E8F0` (near white) on dark backgrounds, `#0D2137` (navy) on light backgrounds.

**Logo Sizes**:
- Full: Icon (32×32px) + wordmark — used in header and landing hero.
- Mark only: Icon (24×24px) — used in mobile header, favicon, tab.
- Compact: Icon (16×16px) — used in footer, admin breadcrumbs.

**SVG Build Notes for Agent**:
- Build the logo as a single `<svg>` component in `src/components/ui/Logo.tsx`.
- Accept a `variant` prop: `"full" | "mark" | "compact"` and a `theme` prop: `"dark" | "light"`.
- The prism shape: equilateral triangle projected isometrically. Left face: `#0D2137`. Top face: `#1E3A5F`. Right face: `#152D4A`. Stroke: `#2D4A6B`, 1px.
- The three rays diverge at 15°, 0°, and -15° from the prism's right vertex.

---

## 4. Loader / Splash Screen

### When It Appears
- On initial app load (before React hydrates).
- On full-page navigation when auth is being checked.
- NOT on route transitions within the authenticated app (use skeleton loaders there).

### Visual Description
**Background**: `#050D1A` — deepest navy, near black.

**Center Element**: The Prism logo mark (isometric prism icon, 64×64px) rendered in its standard colors.

**Animation (GSAP timeline)**:
1. `t=0`: Logo mark fades in from opacity 0, scale 0.8 → 1.0 over 0.4s, ease `power2.out`.
2. `t=0.4`: Three refraction rays animate outward from the prism's right vertex — they draw from length 0 to their full length (80px) over 0.5s using GSAP `drawSVG` or a `strokeDashoffset` trick. The rays stagger: green first (0ms delay), amber (80ms), red (160ms).
3. `t=0.9`: The wordmark "PRISM" fades in, letter by letter using a GSAP `stagger` — each character animates from `opacity: 0, y: 8` to `opacity: 1, y: 0` with 40ms stagger.
4. `t=1.4`: Below the wordmark, a single horizontal scan line (1px tall, full width of the wordmark, `#2D4A6B`) wipes left-to-right over 0.6s using a `scaleX` tween from 0 → 1.
5. `t=2.0`: Entire loader fades out over 0.3s, app content fades in.

**Loading Text** (optional, below the scan line):
```
INITIALIZING SIGNAL ENGINE...
```
In `JetBrains Mono`, 10px, `#4A6785`, uppercase.

**Implementation Notes**:
- Build as `src/components/ui/SplashLoader.tsx`.
- Use a `useEffect` with a minimum display time of 1.8s to prevent flash.
- Mount/unmount controlled by a top-level `isAppReady` state in `App.tsx`.

---

## 5. Global Design Tokens

### Color Palette (Tailwind Config Extension)
```js
// tailwind.config.ts — extend colors
colors: {
  // Brand Primaries
  'prism-navy':       '#050D1A',   // deepest bg
  'prism-navy-2':     '#0D1B2E',   // card bg
  'prism-navy-3':     '#1A2D45',   // elevated card, border
  'prism-navy-4':     '#243650',   // hover states, dividers
  'prism-steel':      '#2D4A6B',   // muted borders
  'prism-blue':       '#1E3A5F',   // secondary bg elements

  // Text
  'prism-text-1':     '#E2E8F0',   // primary text (near white)
  'prism-text-2':     '#94A3B8',   // secondary text
  'prism-text-3':     '#64748B',   // muted/disabled text
  'prism-text-mono':  '#A8C5E8',   // monospaced numbers, data

  // Signal Classification Colors
  'signal-green':     '#16A34A',   // INFORMED MOVE — primary
  'signal-green-bg':  '#052E16',   // INFORMED bg
  'signal-green-muted':'#14532D',  // INFORMED border
  'signal-amber':     '#F59E0B',   // UNCERTAIN — primary
  'signal-amber-bg':  '#1C1407',   // UNCERTAIN bg
  'signal-amber-muted':'#78350F',  // UNCERTAIN border
  'signal-red':       '#DC2626',   // NOISE — primary
  'signal-red-bg':    '#1C0707',   // NOISE bg
  'signal-red-muted': '#7F1D1D',   // NOISE border

  // UI Accents
  'prism-accent':     '#3B82F6',   // interactive blue — links, active states
  'prism-accent-2':   '#1D4ED8',   // hover blue

  // Chart Colors
  'chart-1':          '#3B82F6',
  'chart-2':          '#8B5CF6',
  'chart-3':          '#06B6D4',
  'chart-4':          '#F59E0B',
}
```

### Typography
```js
// tailwind.config.ts — extend fontFamily
fontFamily: {
  mono:  ['JetBrains Mono', 'IBM Plex Mono', 'Fira Code', 'monospace'],
  sans:  ['Inter', 'system-ui', 'sans-serif'],
  display: ['Inter', 'system-ui', 'sans-serif'],
}
```

Import in `index.css`:
```css
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&family=JetBrains+Mono:wght@400;500;700&display=swap');
```

### Typography Scale
| Role | Class | Usage |
|---|---|---|
| Page Title | `font-display text-2xl font-bold text-prism-text-1` | H1 on each page |
| Section Title | `font-sans text-lg font-semibold text-prism-text-1` | Section headings |
| Card Title | `font-sans text-sm font-medium text-prism-text-1` | Market names |
| Data Number | `font-mono text-xl font-bold text-prism-text-mono` | Scores, prices |
| Data Number SM | `font-mono text-sm font-medium text-prism-text-mono` | Liquidity, volume |
| Body | `font-sans text-sm font-normal text-prism-text-2` | Descriptions |
| Caption | `font-sans text-xs font-normal text-prism-text-3` | Timestamps, labels |
| AI Insight | `font-sans text-sm italic text-prism-text-2` | Gemini text |
| Mono Label | `font-mono text-xs uppercase tracking-widest text-prism-text-3` | Section headers, column labels |

### Spacing System
Use Tailwind's default spacing. Custom rules:
- Card padding: `p-4` (16px) mobile, `p-5` (20px) desktop.
- Card gap in feed: `gap-3` (12px).
- Section vertical spacing: `py-8` (32px) mobile, `py-12` (48px) desktop.

### Border Radius
- Cards: `rounded-lg` (8px).
- Chips/badges: `rounded-full`.
- Inputs: `rounded-md` (6px).
- Modals: `rounded-xl` (12px).

### Box Shadow / Elevation
Do NOT use heavy box shadows. Use border-based elevation:
- Level 0 (base): `border border-prism-navy-3`
- Level 1 (card): `border border-prism-steel bg-prism-navy-2`
- Level 2 (elevated card): `border border-prism-navy-4 bg-prism-navy-3 shadow-[0_2px_16px_rgba(0,0,0,0.4)]`
- Level 3 (modal): `border border-prism-steel bg-prism-navy-2 shadow-[0_8px_40px_rgba(0,0,0,0.6)]`

---

## 6. Global Layout & Navigation Shell

### Shell Structure (Authenticated App)
```
┌─────────────────────────────────────────────────────┐
│  HEADER (fixed, 56px height)                        │
├──────────────────────────────────────────────────────┤
│  SIDEBAR      │  MAIN CONTENT AREA                  │
│  (240px,      │  (flex-1, scrollable)               │
│  desktop only)│                                     │
│               │                                     │
│               │                                     │
├──────────────────────────────────────────────────────┤
│  BOTTOM NAV (mobile only, fixed, 56px height)       │
└─────────────────────────────────────────────────────┘
```

### Header (`src/components/layout/Header.tsx`)

**Height**: 56px fixed, `sticky top-0 z-50`.
**Background**: `bg-prism-navy/95 backdrop-blur-sm border-b border-prism-navy-3`.

**Left section (desktop)**:
- `Logo` component in `full` variant, `dark` theme.
- Below the logo wordmark, a tiny badge: `BETA` in `font-mono text-[9px] uppercase tracking-widest bg-prism-navy-3 text-prism-text-3 px-1.5 py-0.5 rounded`.

**Left section (mobile)**:
- `Logo` component in `mark` variant only (icon, no wordmark).

**Center section (desktop only)**:
- Live pulse indicator: a 6px circle that breathes (CSS pulse animation, `bg-signal-green opacity-80`), followed by text: `LIVE · last updated {X}s ago` in `font-mono text-xs text-prism-text-3`. This updates every second as a countdown to the next 30s refresh.

**Right section**:
- Desktop: Three nav links inline: `Dashboard`, `Performance`, `Methodology`. Style: `font-sans text-sm text-prism-text-2 hover:text-prism-text-1 transition-colors px-3 py-1`. Active state: `text-prism-text-1 border-b border-prism-accent`.
- Avatar button: A 32×32px circle with the user's initials (from cookie session), `bg-prism-navy-3 border border-prism-steel font-mono text-xs text-prism-text-1`. Clicking opens a dropdown.
- Admin users: An additional `ADMIN` chip before avatar: `font-mono text-[10px] uppercase bg-prism-navy-3 border border-prism-steel text-prism-text-2 px-2 py-0.5 rounded-full`. Clicking navigates to `/admin`.

**Avatar Dropdown**:
- Floats from the avatar button, `z-50`, `min-w-[180px]`.
- Background: `bg-prism-navy-2 border border-prism-steel rounded-lg shadow-[0_8px_40px_rgba(0,0,0,0.6)]`.
- Items: `Profile`, `Settings` (greyed out, not built yet), divider, `Sign Out`.
- `Sign Out` is `text-signal-red`. On click: calls `POST /auth/logout` which clears the HTTP-only cookie, then navigates to `/login`.

### Sidebar (`src/components/layout/Sidebar.tsx`)

**Width**: 240px, fixed. Only rendered `md:block hidden`.
**Background**: `bg-prism-navy border-r border-prism-navy-3`.
**Position**: `fixed left-0 top-[56px] h-[calc(100vh-56px)] overflow-y-auto`.

**Nav Links** (stacked, full width):
Each link: `flex items-center gap-3 px-4 py-2.5 text-sm font-sans text-prism-text-2 hover:bg-prism-navy-2 hover:text-prism-text-1 transition-colors rounded-md mx-2`.
Active state: `bg-prism-navy-3 text-prism-text-1 border-l-2 border-prism-accent`.

Links (with Lucide icons):
- `Activity` icon → `Dashboard` → `/dashboard`
- `TrendingUp` icon → `Performance` → `/performance`
- `BookOpen` icon → `Methodology` → `/methodology`

**Sidebar Stats Block** (at the bottom of the nav links, `mt-6`):
A small block with today's stats, auto-updated by TanStack Query.
```
┌────────────────────────┐
│  TODAY'S SIGNALS       │  ← font-mono text-[10px] uppercase tracking-widest text-prism-text-3
│                        │
│  24    total           │  ← number in font-mono text-xl bold text-prism-text-1
│  █████░░░░░            │  ← thin progress bar
│  10    informed        │  ← signal-green text
│  8     uncertain       │  ← signal-amber text
│  6     noise           │  ← signal-red text
└────────────────────────┘
```
The block has `bg-prism-navy-2 border border-prism-navy-3 rounded-lg p-3 mx-2`.

### Bottom Navigation (Mobile, `src/components/layout/BottomNav.tsx`)

**Only rendered** `md:hidden`, `fixed bottom-0 left-0 right-0 z-50`.
**Height**: 56px.
**Background**: `bg-prism-navy/95 backdrop-blur-sm border-t border-prism-navy-3`.

Four icons centered with labels below:
- `Activity` → Dashboard
- `TrendingUp` → Performance
- `BookOpen` → Methodology
- Avatar circle → Profile dropdown (mobile sheet)

Each item: `flex flex-col items-center gap-1 text-[10px] font-sans`.
Active: icon in `text-prism-accent`, label in `text-prism-text-1`.
Inactive: icon and label in `text-prism-text-3`.

---

## 7. Page: Landing Page (`/`)

> This is the most important page. It must immediately communicate what Prism is and create professional credibility. It is bespoke, scroll-driven, and uses GSAP + Three.js for the hero, and D3 for the signal visualization section. This is NOT a generic SaaS landing page.

**Route**: `/` — accessible without auth. If user is authenticated, a `LAUNCH DASHBOARD →` button is shown in the nav instead of `Sign In`.

**File**: `src/pages/Landing.tsx`

---

### 7.0 Navbar (Landing-specific, not the app shell)

**Height**: 64px, `fixed top-0 left-0 right-0 z-50`.
**Background**: `transparent` by default, transitions to `bg-prism-navy/95 backdrop-blur-sm border-b border-prism-navy-3` after 80px of scroll (GSAP ScrollTrigger on `scrollY`).
**Padding**: `px-6` mobile, `px-12` desktop.

Left: `Logo` full variant, dark theme.
Right:
- `Methodology` link — `font-sans text-sm text-prism-text-2 hover:text-prism-text-1 transition-colors`.
- `Sign In` → `/login` — same text style.
- `Get Access` → `/register` — `bg-prism-accent text-white text-sm font-sans font-medium px-4 py-1.5 rounded-md hover:bg-prism-accent-2 transition-colors`.

Mobile: hamburger (`Menu` Lucide icon), opens a drawer from the right (`bg-prism-navy-2 border-l border-prism-steel w-[240px]`) with the same links stacked.

---

### 7.1 Hero Section

**Full viewport height** (`min-h-screen`), `relative overflow-hidden`, `bg-prism-navy`.

**Three.js Background Canvas** (`src/components/landing/HeroCanvas.tsx`):
- Fills the entire section as a `position: absolute inset-0` canvas.
- Renders a **particle field**: ~600 small particles (`THREE.Points`), each 1.2px, in `#1E3A5F` to `#2D4A6B` color range. Particles slowly drift (sinusoidal motion on a per-particle phase offset). Think: deep space, but orderly — not chaotic.
- A single **refraction simulation**: Three diverging beams of light shoot from a central prism model. The prism is a simple `THREE.ConeGeometry(0.3, 0.8, 3)` (a triangle when viewed front-on) rendered in wireframe with edges in `#2D4A6B`. From its right tip, three `THREE.Line` geometries extend: green (top), amber (middle), red (bottom). These lines animate in on page load using GSAP.
- The camera gently rotates on the Y-axis at `0.0003` radians per frame, giving depth.
- On mouse move: the prism object slightly tilts toward cursor position using lerp (subtle, max 5° rotation).
- Performance: Use `requestAnimationFrame` with a `resize` observer. Pause when tab is hidden.
- **Mobile**: Reduce particles to 200, disable mouse tracking.

**Hero Content** (centered, `relative z-10`, `flex flex-col items-center text-center`):

```
[ small eyebrow label ]
BUILT ON MARKET MICROSTRUCTURE THEORY

[ main headline — two lines ]
Separate Signal
from Noise.

[ sub-headline ]
Prism quantifies whether a prediction market
price move is informed trading, speculation, or
noise — in real time.

[ score demo strip ]
[ CTA buttons ]
```

**Eyebrow Label**:
`font-mono text-xs uppercase tracking-[0.3em] text-prism-text-3`.
Animate in: `opacity: 0, y: -12` → `opacity: 1, y: 0`, 0.5s, ease `power2.out`, delay 0.2s after canvas loads.

**Main Headline**:
`font-display text-5xl md:text-7xl lg:text-8xl font-bold text-prism-text-1 leading-[0.95]`.
"Signal" is highlighted with a subtle `text-prism-accent` or kept white — design preference is white for restraint.
Animate in: words stagger in from `opacity: 0, y: 40` → `opacity: 1, y: 0` with 0.12s stagger per word, 0.4s ease `power3.out`, starting at t=0.5s.

**Sub-headline**:
`font-sans text-base md:text-lg text-prism-text-2 max-w-md mt-4`.
Animate in: fade + translate, t=0.9s.

**Hero Score Strip** (this is the killer differentiator widget in the hero):
A horizontal strip showing 3 example signal cards in miniature, demonstrating the three classifications.
Renders as a `flex gap-4` row. On mobile, this is a horizontal scroll carousel.

Each mini-card (`min-w-[180px] md:w-[220px]`):
```
┌──────────────────────┐
│ [●86] INFORMED MOVE  │  ← score badge + chip
│ Osun Election        │  ← market title, truncated
│ 0.52 → 0.64 (+12%)  │  ← price delta, font-mono
│ NGN 2.1M · 842 ord  │  ← data row
└──────────────────────┘
```
Style: `bg-prism-navy-2 border border-signal-green-muted rounded-lg p-3` (green for INFORMED), amber/red variants for others.
These cards animate in with a stagger: scale from 0.9 → 1, opacity 0 → 1, 0.4s ease, 0.15s stagger starting at t=1.1s.
On hover: `border-signal-green` brightens, slight `translateY(-2px)` transition.

**CTA Buttons**:
Primary: `Get Started` → `/register`. Style: `bg-prism-accent hover:bg-prism-accent-2 text-white font-sans font-medium text-sm px-6 py-2.5 rounded-md transition-colors`.
Secondary: `View Live Signals` → `/dashboard`. Style: `border border-prism-steel text-prism-text-2 hover:border-prism-text-2 hover:text-prism-text-1 font-sans text-sm px-6 py-2.5 rounded-md transition-colors`.
`flex gap-3 mt-8`.

**Scroll Indicator** (bottom of hero, centered):
A small animated chevron-down icon in `text-prism-text-3`, CSS bounce animation. Disappears after user scrolls.

---

### 7.2 Problem Statement Section

**Background**: `bg-prism-navy-2`.
**Padding**: `py-20 px-6 md:px-16`.

**Section Label** (eyebrow):
`THE PROBLEM` in `font-mono text-xs uppercase tracking-[0.3em] text-prism-text-3 mb-3`.

**Headline**:
`One move. Two completely different realities.`
`font-display text-3xl md:text-4xl font-bold text-prism-text-1 max-w-2xl`.

**Two-Column Comparison Cards** (`grid grid-cols-1 md:grid-cols-2 gap-4 mt-10 max-w-4xl mx-auto`):

Left card (bad scenario):
```
┌──────────────────────────────────┐
│  MANIPULATION TRAP               │  ← font-mono text-xs text-signal-red uppercase
│                                  │
│  Market jumps +20%               │
│  Liquidity: NGN 40,000           │  ← all in font-mono
│  Orders: 3                       │
│  Volume spike: 1.1x              │
│                                  │
│  Score: [22] NOISE               │  ← red score badge
│                                  │
│  "One person moved this market.  │  ← italic text-prism-text-3
│   You just got trapped."         │
└──────────────────────────────────┘
```
Border: `border border-signal-red-muted bg-signal-red-bg`.

Right card (good scenario):
```
┌──────────────────────────────────┐
│  INFORMED MOVE                   │  ← font-mono text-xs text-signal-green uppercase
│                                  │
│  Market moves +8%                │
│  Liquidity: NGN 2,100,000        │
│  Orders: 842                     │
│  Volume spike: 3.2x              │
│                                  │
│  Score: [86] INFORMED MOVE       │  ← green score badge
│                                  │
│  "Broad participation. Deep      │  ← italic text-prism-text-2
│   liquidity. Pay attention."     │
└──────────────────────────────────┘
```
Border: `border border-signal-green-muted bg-signal-green-bg`.

**GSAP ScrollTrigger**: Cards animate from `opacity: 0, x: -40` (left) and `opacity: 0, x: 40` (right) → `opacity: 1, x: 0` as they enter viewport. Trigger: `start: "top 80%"`.

**Below cards**: A single line of text:
`Retail traders on Bayse cannot tell the difference. Prism can.`
`font-sans text-base text-prism-text-2 text-center mt-8 max-w-xl mx-auto`.

---

### 7.3 How It Works Section

**Background**: `bg-prism-navy` (alternating from previous section).
**Padding**: `py-20 px-6 md:px-16`.

**Eyebrow**: `HOW PRISM WORKS`.

**Headline**: `Four factors. One score. Instant clarity.`

**Step Flow Diagram** (built with D3.js, `src/components/landing/ScoreFlowDiagram.tsx`):
This is a horizontal flow diagram showing the pipeline.

```
[MARKET MOVE]  →  [PRICE DELTA]  \
                  [LIQUIDITY  ]   → [SCORE 0-100]  → [NOISE/UNCERTAIN/INFORMED]  → [AI INSIGHT]
                  [VOLUME     ]  /
                  [ORDERS     ]
```

D3 renders this as an SVG with:
- Rectangles for each node, rounded corners, `fill: #0D1B2E, stroke: #2D4A6B`.
- Animated connection lines: bezier curves in `stroke: #3B82F6, opacity: 0.4`.
- On scroll into view (IntersectionObserver), animate lines drawing from left to right using `stroke-dashoffset`.
- Score node in center: a 70px circle with a radial gradient (navy center → slight blue edge), number "0–100" inside in font-mono bold.
- The three output boxes use signal colors: green, amber, red borders.

**Below the diagram**, a four-factor grid (`grid grid-cols-2 md:grid-cols-4 gap-4 mt-12`):
Each card:
```
┌──────────────────┐
│  [icon]          │
│  PRICE DELTA     │  ← font-mono text-xs uppercase text-prism-text-3
│                  │
│  How large is    │  ← font-sans text-xs text-prism-text-3
│  the probability │
│  change?         │
│                  │
│  Weight: 40%     │  ← font-mono text-xs text-prism-accent
└──────────────────┘
```
Icons (Lucide): `TrendingUp` (price), `Droplets` (liquidity), `Zap` (volume), `Users` (orders).
Border: `border border-prism-navy-3 bg-prism-navy-2 rounded-lg p-4`.

---

### 7.4 Live Signal Preview Section

**Background**: `bg-prism-navy-2`.
**Padding**: `py-20 px-6 md:px-16`.

**Eyebrow**: `LIVE SIGNAL FEED`.

**Headline**: `See what Prism is detecting right now.`

This section renders **real data** from `GET /signals/latest` using TanStack Query (unauthenticated endpoint), showing the top 4 signals. This is the "show, don't tell" moment on the landing page.

If the user is not logged in, the signals are shown but the AI insight is blurred (`blur-sm` on the AI text) with a `Sign in to see full insights` prompt overlaid.

Layout: `grid grid-cols-1 md:grid-cols-2 gap-4 max-w-5xl mx-auto mt-10`.

Each card is a fully rendered `SignalCard` component (see §19) but in a slightly simplified "preview" mode.

Below the grid: `View all signals →` link styled as a subtle button → `/dashboard` (redirects to `/login` if not authenticated).

**Loading state**: 4 `SignalCardSkeleton` components.

---

### 7.5 The Science Section (Quant Credibility)

**Background**: `bg-prism-navy`.
**Padding**: `py-20 px-6 md:px-16`.

**Eyebrow**: `THE QUANT FOUNDATION`.

**Headline**: `Institutional-grade logic. Built for emerging markets.`

**Three columns** (`grid grid-cols-1 md:grid-cols-3 gap-6 mt-10`):

Column 1:
- Title: `Kyle's Lambda`
- `font-mono text-[10px] uppercase text-prism-text-3 mb-1`: `MARKET MICROSTRUCTURE`
- Body: Short explanation (2 sentences) of price impact and why thin markets are penalized.
- Bottom tag: `Price Impact · Order Flow`

Column 2:
- Title: `Amihud Illiquidity`
- Tag: `LIQUIDITY THEORY`
- Body: Ratio of price change to volume. High ratio = easily moved. Prism uses it to detect shallow markets.
- Bottom tag: `Depth · Resilience`

Column 3:
- Title: `Adverse Selection`
- Tag: `INFORMATION ASYMMETRY`
- Body: In thin markets, you might be trading against someone who knows more. Prism quantifies this risk.
- Bottom tag: `Informed Flow · Signal Quality`

Card style: `border border-prism-navy-3 bg-prism-navy-2 rounded-lg p-5`.

---

### 7.6 Social Proof / Accuracy Strip

**Background**: A subtle horizontal band, `bg-prism-navy-3`.
**Height**: ~120px.
**Content**: Three stat numbers centered in a row:

```
[  64%  ]     [  847  ]     [  3x   ]
Directional   Signals        Outperforms
accuracy on   tracked        random on
INFORMED      this month     high-score
signals                      signals
```

Numbers in `font-mono text-4xl font-bold text-prism-text-1`.
Labels in `font-sans text-xs text-prism-text-3 text-center`.
Dividers between stats: `border-r border-prism-navy-4`.

**GSAP CountUp**: Numbers count up from 0 when they scroll into view. Use GSAP's `fromTo` on a custom counter state.

---

### 7.7 CTA Section (Final)

**Background**: `bg-prism-navy-2`.
**Padding**: `py-24`.

**Centered block**:
- Logo mark (48×48px, centered).
- Headline: `Ready to trade with clarity?` — `font-display text-3xl md:text-4xl font-bold text-prism-text-1 text-center mt-4`.
- Sub: `Prism is free to use. No credit card required.` — `font-sans text-sm text-prism-text-3 mt-2 text-center`.
- CTA: `Get Access` → `/register` — large primary button, `px-8 py-3 text-base mt-6`.

**GSAP**: Entire block animates in from `opacity: 0, y: 30` as it enters viewport.

---

### 7.8 Footer

**Background**: `bg-prism-navy border-t border-prism-navy-3`.
**Padding**: `py-10 px-6 md:px-16`.

Left: Logo compact + `PRISM Signal Intelligence · © 2025`. `font-sans text-xs text-prism-text-3`.
Right: Links: `Methodology`, `GitHub`, `Sign In`. `font-sans text-xs text-prism-text-3 hover:text-prism-text-2`.

---

## 8. Page: Auth — Login (`/login`)

**File**: `src/pages/Login.tsx`
**Route**: `/login` — redirect to `/dashboard` if already authenticated.

### Layout
Full-screen split layout on desktop (`grid grid-cols-2`). Single column on mobile.

**Left Panel** (desktop only, `col-span-1 bg-prism-navy-2 border-r border-prism-navy-3 flex flex-col justify-between p-12`):
- Top: Logo full, dark theme.
- Middle: A large quote block:
  ```
  "A prism takes white light
   and reveals what's hidden
   inside complexity."
  ```
  In `font-display text-2xl font-light text-prism-text-2 italic leading-relaxed`.
  Below it: `—  Signal Intelligence Engine` in `font-mono text-xs text-prism-text-3`.
- Bottom: The three-ray icon (just the rays, no prism body) rendered as a small SVG, with each ray labeled: `INFORMED · UNCERTAIN · NOISE`. Each label in its signal color.

**Right Panel** (`col-span-1 flex flex-col justify-center items-center px-8 md:px-16 bg-prism-navy`):

**Form block** (`w-full max-w-sm`):

Eyebrow: `PRISM` in `font-mono text-xs uppercase tracking-[0.3em] text-prism-text-3 mb-6`.

Headline: `Sign in` in `font-display text-2xl font-bold text-prism-text-1 mb-1`.
Sub: `Access your signal dashboard` in `font-sans text-sm text-prism-text-3 mb-8`.

**Form fields**:
Label: `font-sans text-xs font-medium text-prism-text-2 mb-1.5 uppercase tracking-wide`.
Input: `w-full bg-prism-navy-2 border border-prism-steel rounded-md px-3 py-2.5 font-mono text-sm text-prism-text-1 placeholder:text-prism-text-3 focus:outline-none focus:border-prism-accent transition-colors`.

- Email input: type `email`, placeholder `you@example.com`.
- Password input: type `password`, placeholder `••••••••`. Right-side toggle icon (`Eye`/`EyeOff` Lucide) to show/hide.

**Submit Button**:
`Sign In` — full width, `bg-prism-accent hover:bg-prism-accent-2 text-white font-sans font-medium text-sm py-2.5 rounded-md transition-colors mt-2`.
Loading state: spinner icon (`Loader2` Lucide, animate-spin) + `Verifying...` text, button disabled.

**Error State**:
Below the submit button: `bg-signal-red-bg border border-signal-red-muted rounded-md px-3 py-2 text-signal-red text-xs font-sans`.
Error message: `Invalid credentials. Please try again.`
Animate in: `opacity: 0, y: -8` → `opacity: 1, y: 0`, 0.2s.

**Footer links** (below form):
`Don't have an account?` + `Create one` link → `/register`. `font-sans text-xs text-prism-text-3`.

**Auth flow**: 
1. User submits email + password.
2. POST to `/auth/login` with JSON body.
3. Server responds with Set-Cookie header (HTTP-only, Secure, SameSite=Strict) and user payload.
4. On success: navigate to `/dashboard`.
5. On failure: show error message.
6. Never store token in localStorage or sessionStorage. Cookie is managed entirely by the browser and server.

---

## 9. Page: Auth — Register (`/register`)

**File**: `src/pages/Register.tsx`
**Route**: `/register`

### Layout
Same split layout as Login. Left panel is identical.

**Right Panel Form block**:

Headline: `Create account`.
Sub: `Start filtering signal from noise.`

**Fields** (stacked, same input style as login):
1. **Full Name** — text input, placeholder `Your name`.
2. **Email** — email input.
3. **Password** — password input with show/hide toggle. Below: a password strength meter (4 thin bars, CSS-colored: `bg-signal-red`, `bg-signal-amber`, `bg-signal-green`, based on strength). This is a cosmetic only indicator, no external library.
4. **Confirm Password** — password input.

**Submit Button**: `Create Account`.
Loading: spinner + `Creating account...`.

**Error handling**:
- Email already exists: `An account with this email already exists.`
- Password mismatch: `Passwords do not match.` (validated client-side before submit).
- Server error: generic `Something went wrong. Please try again.`

**Success flow**:
1. POST to `/auth/register`.
2. Server creates user, sets session cookie.
3. Navigate to `/dashboard` with a brief success toast.

**Footer**: `Already have an account?` + `Sign in` → `/login`.

---

## 10. Page: Dashboard — Signal Feed (`/dashboard`)

**File**: `src/pages/Dashboard.tsx`
**Route**: `/dashboard` — auth required. Redirect to `/login` if no valid session.

This is the **core product page**. It must be information-dense but scannable.

### Layout (Desktop)
```
┌─────────────────────────────────────────────────────────────┐
│  HEADER (56px)                                              │
├──────────────────┬──────────────────────────────────────────┤
│  SIDEBAR (240px) │  PAGE CONTENT (flex-1)                  │
│                  │  ┌────────────────────────────────────┐  │
│                  │  │ PAGE HEADER                        │  │
│                  │  ├────────────────────────────────────┤  │
│                  │  │ FILTER BAR                         │  │
│                  │  ├──────────────────┬─────────────────┤  │
│                  │  │ SIGNAL FEED      │ STATS PANEL     │  │
│                  │  │ (flex-1)         │ (280px)         │  │
│                  │  │                  │                 │  │
│                  │  └──────────────────┴─────────────────┘  │
└──────────────────┴──────────────────────────────────────────┘
```

### Layout (Mobile)
Single column. Stats panel collapses into a horizontal scrollable strip at the top. Filter bar stacks vertically. Signal feed fills full width.

### 10.1 Page Header

```
Signal Feed              ← font-display text-xl font-bold text-prism-text-1
Live · 24 signals today  ← font-mono text-xs text-prism-text-3, with live pulse dot
```

Right side: A `Refresh` button (icon only: `RefreshCw` Lucide, 16px, `text-prism-text-3 hover:text-prism-text-1`). On click: invalidates TanStack Query cache and triggers immediate refetch.

### 10.2 Filter Bar

`flex flex-wrap gap-2 py-3 border-b border-prism-navy-3`.

**Filter chips** (toggle buttons, multi-select):
- `ALL` (default active)
- `INFORMED` — when active: `bg-signal-green-bg border border-signal-green-muted text-signal-green`.
- `UNCERTAIN` — amber variant.
- `NOISE` — red variant.
Inactive: `bg-prism-navy-2 border border-prism-navy-3 text-prism-text-3 hover:text-prism-text-2`.
Style: `font-mono text-xs uppercase px-3 py-1 rounded-full cursor-pointer transition-colors`.

**Sort dropdown** (right-aligned):
`Sort by:` label + select: `Highest Score | Recent | Market Name`.
Style: `bg-prism-navy-2 border border-prism-steel rounded-md px-2 py-1 text-xs font-mono text-prism-text-2 focus:outline-none focus:border-prism-accent`.

### 10.3 Signal Feed

`flex flex-col gap-3` containing `SignalCard` components.

**Auto-refresh**: TanStack Query polls every 30s. When new data arrives, a subtle banner appears at the top of the feed:
```
↑ 3 new signals — Click to refresh
```
`bg-prism-navy-3 border border-prism-steel text-prism-text-2 text-xs font-mono py-2 px-4 rounded-md cursor-pointer text-center`. On click: scroll to top + apply new data.

**SignalCard** (see full spec in §19 Component Library).

**Infinite scroll / pagination**: Load initial 20 signals. On scroll to within 200px of bottom, append next 20 via TanStack Query `fetchNextPage`.

**Empty state** (when filters return nothing):
```
[Activity icon, 32px, text-prism-text-3]
No signals matching this filter.
Prism is monitoring Bayse markets — signals will appear here as they are detected.
```
Centered, `py-16`, text in `font-sans text-sm text-prism-text-3`.

### 10.4 Stats Panel (Desktop Sidebar, 280px)

`sticky top-[72px]` (accounts for fixed header). Height auto, `flex flex-col gap-4`.

**Panel 1: Today's Overview**
```
┌────────────────────────────┐
│  TODAY'S OVERVIEW          │  ← section label
│                            │
│  24        signals total   │
│  ──────────────────────    │
│  10 ████   INFORMED 42%    │
│   8 ███    UNCERTAIN 33%   │
│   6 ██     NOISE     25%   │
└────────────────────────────┘
```
Progress bars: `h-1.5 rounded-full bg-prism-navy-4`. Fill: signal color, width = percentage.

**Panel 2: Top Signal Right Now**
The single highest-scoring signal from the current feed, in a compact format:
```
┌────────────────────────────┐
│  TOP SIGNAL                │
│                            │
│  [●86]                     │
│  Osun Election Winner      │
│  0.52 → 0.64  +12%        │
│  View →                    │
└────────────────────────────┘
```
`border border-signal-green-muted bg-signal-green-bg`. Clicking navigates to `/market/:id`.

**Panel 3: Market Monitor**
A compact list of the top 5 markets by liquidity that are currently being tracked.
```
┌────────────────────────────┐
│  ACTIVE MARKETS            │
│                            │
│  Osun Election     2.1M    │
│  AFCON Winner      890k    │
│  Bitcoin >60k      450k    │
│  Tinubu Approval   320k    │
│  EPL Top Scorer    210k    │
└────────────────────────────┘
```
Numbers in `font-mono text-xs text-prism-text-mono`.
Clicking any market navigates to `/market/:id`.

### 10.5 Mobile Stats Strip

`flex overflow-x-auto gap-3 pb-2 border-b border-prism-navy-3` — appears ABOVE the filter bar on mobile.
Three compact stat chips:
- `24 TOTAL` 
- `10 INFORMED` (green bg)
- `8 UNCERTAIN` (amber bg)

Each: `flex-shrink-0 px-3 py-1.5 rounded-full border font-mono text-xs`.

---

## 11. Page: Market Detail (`/market/:id`)

**File**: `src/pages/MarketDetail.tsx`
**Route**: `/market/:id` — auth required.

This page gives a deep-dive view of a single prediction market and all signals detected for it.

### 11.1 Page Header

Back button: `← Back to Feed` — `font-sans text-sm text-prism-text-3 hover:text-prism-text-1 flex items-center gap-1`.

Market title: `font-display text-2xl font-bold text-prism-text-1 mt-2`.
Category badge: `font-mono text-[10px] uppercase tracking-wide px-2 py-0.5 bg-prism-navy-3 border border-prism-steel rounded-full text-prism-text-3 ml-2`.

### 11.2 Market Snapshot Row

`flex flex-wrap gap-4 items-center mt-4 py-4 border-y border-prism-navy-3`.

Stats:
- `Current Probability`: Large, `font-mono text-3xl font-bold text-prism-text-1`.
- `Liquidity`: `font-mono text-sm text-prism-text-mono` with label.
- `Total Orders`: same.
- `Last Updated`: `font-mono text-xs text-prism-text-3`.

### 11.3 Probability Chart (D3.js)

`src/components/charts/ProbabilityChart.tsx`

A time-series line chart of probability over time, using the Snapshot data from `GET /markets/:id`.

**Spec**:
- Width: full container width. Height: 200px desktop, 160px mobile. Responsive via ResizeObserver.
- X-axis: time (last 24 hours). Y-axis: 0% to 100%.
- Line: `stroke: #3B82F6, strokeWidth: 2`. Smooth curve (D3 `curveMonotoneX`).
- Area fill: gradient from `rgba(59,130,246,0.15)` at top to `rgba(59,130,246,0)` at bottom.
- Signal markers: For each signal detected in this market, render a small vertical dashed line on the chart at that timestamp, colored by classification (green/amber/red). On hover, a tooltip shows the signal score.
- X-axis labels: `font-mono text-[10px] text-prism-text-3`.
- Y-axis labels: Percentage values.
- Background grid lines: `stroke: #1A2D45, strokeWidth: 1, opacity: 0.5`.
- Tooltip: `bg-prism-navy-2 border border-prism-steel rounded-md px-2 py-1 text-xs font-mono text-prism-text-1`.

### 11.4 Signals History Table

All signals ever detected for this market, newest first.

**Table header**: `SCORE | MOVE | LIQUIDITY | VOLUME | ORDERS | TIME | CLASSIFICATION`
Style: `font-mono text-[10px] uppercase tracking-wide text-prism-text-3 border-b border-prism-navy-3 py-2`.

**Each row** (`border-b border-prism-navy-3/50 py-3 hover:bg-prism-navy-2 transition-colors`):
- Score badge (compact, 32px circle).
- Move: `+12%` in signal color.
- Liquidity, volume ratio, orders: `font-mono text-xs text-prism-text-mono`.
- Relative timestamp: `2 min ago` in `font-mono text-xs text-prism-text-3`.
- Classification chip.

Clicking a row expands it (smooth `max-height` CSS transition, 0.2s) to show the full AI insight below.

**Empty**: `No signals detected for this market yet.`

### 11.5 AI Insight Panel

If the latest signal has an AI insight (score ≥ 70), it renders in a highlighted block:

```
┌──────────────────────────────────────────────┐
│  GEMINI ANALYSIS                             │  ← font-mono text-[10px] text-prism-text-3
│  [sparkle icon]                              │
│                                              │
│  "Strong participation and deep liquidity    │  ← italic text-sm text-prism-text-2
│   suggest this move reflects informed        │
│   repositioning, not short-term              │
│   speculation."                              │
└──────────────────────────────────────────────┘
```
Border: `border border-signal-green-muted bg-signal-green-bg rounded-lg p-4`.

---

## 12. Page: Performance Tracker (`/performance`)

**File**: `src/pages/Performance.tsx`
**Route**: `/performance` — auth required.

This is the credibility page. It must look rigorous and data-driven.

### 12.1 Page Header

Title: `Signal Performance`
Sub: `Historical accuracy of Prism signal classifications.`

### 12.2 Summary Metrics Row

`grid grid-cols-2 md:grid-cols-4 gap-4 mt-6`.

Each metric card (`bg-prism-navy-2 border border-prism-navy-3 rounded-lg p-4`):
1. **Total Signals Tracked**: count, `font-mono text-3xl font-bold`.
2. **Resolved Signals**: those with outcome data.
3. **INFORMED Accuracy**: `64%` in `text-signal-green`.
4. **vs. Random Baseline**: `+14pp` over random 50%.

### 12.3 Score Distribution Chart

`src/components/charts/ScoreDistribution.tsx` — D3 histogram.

**Spec**:
- X-axis: Score buckets (0–10, 10–20, ... 90–100).
- Y-axis: Number of signals.
- Bars colored by classification: 0–39 = red, 40–69 = amber, 70–100 = green.
- Width: full container. Height: 180px.
- Hover tooltip: score bucket + count + % of total.
- Axis labels: `font-mono text-[10px] text-prism-text-3`.

### 12.4 Performance Table

Full-width table with all resolved signals.

**Columns**: `MARKET | SCORE | CLASS | DIRECTION CALLED | PROB @ SIGNAL | PROB +1HR | PROB +2HR | OUTCOME`

**Column header style**: `font-mono text-[10px] uppercase tracking-wide text-prism-text-3`.

**Each row**:
- Market: `font-sans text-sm text-prism-text-1` (truncated to 30 chars, full in tooltip).
- Score: compact badge.
- Classification chip.
- Direction called: `▲ YES` or `▼ NO` in signal green/red.
- Probabilities: `font-mono text-xs text-prism-text-mono`.
- Outcome: `CORRECT` in green, `REVERSED` in red, `INCONCLUSIVE` in amber — all as chips.

**Row hover**: `hover:bg-prism-navy-2`.

**Sorting**: Clicking column headers sorts (ascending/descending). Active sort column shows `▲`/`▼` icon.

**Pagination**: 20 per page. Pagination controls at bottom: `← Previous | Page 1 of 4 | Next →`. `font-mono text-xs`.

### 12.5 Accuracy Over Time Chart

A second D3 chart below the table.

Rolling 7-day accuracy rate for INFORMED MOVE signals. X-axis: date. Y-axis: 0–100%. Line chart with same styling as ProbabilityChart. A horizontal dashed reference line at 50% (random baseline), labeled `Random (50%)` in `text-prism-text-3`.

---

## 13. Page: About / Methodology (`/methodology`)

**File**: `src/pages/Methodology.tsx`
**Route**: `/methodology` — public page, no auth required.

This is the explanation page — it builds trust with quant-minded users. Reads like a high-quality technical document, but not a wall of text.

### Layout
`max-w-3xl mx-auto px-6 py-12` — centered, readable width.

### Sections (top-to-bottom):

**Section 1: Hero**
Headline: `How Prism Works`
Sub: `A plain-language guide to the Signal Strength Score and why it matters.`

**Section 2: The Problem** — 3–4 paragraphs with a pull-quote block:
```
┌──────────────────────────────────────────────┐
│  "In low-liquidity markets, most price       │
│   movements are meaningless. Prism           │
│   quantifies the difference between real     │
│   and fake moves."                           │
└──────────────────────────────────────────────┘
```
Style: `border-l-4 border-prism-accent pl-4 italic text-prism-text-2 my-6`.

**Section 3: The Four Factors** — using the same four-factor cards from the landing page, but with expanded descriptions (3–4 sentences each). Include the actual formula in a code block:
```
signal-score = 100 × (
  0.40 × MoveFactor
  0.25 × LiquidityFactor
  0.20 × VolumeFactor
  0.15 × OrderFactor
)
```
Style: `bg-prism-navy-2 border border-prism-navy-3 rounded-lg p-4 font-mono text-sm text-prism-text-mono my-4`.

**Section 4: The Classification Thresholds** — a simple table:

| Score | Class | Meaning |
|---|---|---|
| 0–39 | NOISE | Thin market or manipulation |
| 40–69 | UNCERTAIN | Monitor, insufficient evidence |
| 70–100 | INFORMED MOVE | Strong participation, reliable |

Table style: `w-full border border-prism-navy-3 rounded-lg overflow-hidden`. Header: `bg-prism-navy-3 font-mono text-xs uppercase text-prism-text-3`. Cells: `font-sans text-sm text-prism-text-2 border-b border-prism-navy-3/50 py-2 px-4`.

**Section 5: The AI Layer** — explain Gemini receives quantitative data, not news. One sentence: *"The AI interprets numbers, not headlines."*

**Section 6: What Prism Is Not** — a brief bulleted list (this is the one place a list is appropriate — it's a factual list).

**Section 7: References** — academic citations in a smaller-font block:
Kyle (1985), Amihud (2002), etc. Style: `font-mono text-xs text-prism-text-3 border-t border-prism-navy-3 pt-6 mt-8`.

---

## 14. ADMIN: Admin Layout Shell

**Route prefix**: `/admin` — admin role required. Redirect to `/dashboard` if user is not admin role (determined from cookie session payload).

**Admin role check**: Backend sets a `role` field in the session cookie payload. Frontend reads this from a `/auth/me` endpoint response on app init and stores in React Context. If `role !== 'admin'`, all `/admin` routes redirect to `/dashboard`.

### Admin Shell Layout
```
┌─────────────────────────────────────────────────────────────┐
│  ADMIN HEADER (56px)                                        │
├──────────────────┬──────────────────────────────────────────┤
│  ADMIN SIDEBAR   │  ADMIN CONTENT AREA                     │
│  (240px)         │                                         │
└──────────────────┴──────────────────────────────────────────┘
```

**Admin Header** (`src/components/admin/AdminHeader.tsx`):
- Same structure as user header.
- Additional badge: `ADMIN CONSOLE` in `font-mono text-[10px] uppercase bg-signal-amber-bg border border-signal-amber-muted text-signal-amber px-2 py-0.5 rounded-full ml-3`.
- Breadcrumb path below the logo: `Admin / {current page}` in `font-mono text-xs text-prism-text-3`.

**Admin Sidebar** (`src/components/admin/AdminSidebar.tsx`):
- Same structural style as user sidebar.
- Links:
  - `LayoutDashboard` → Admin Overview → `/admin`
  - `Globe` → Markets → `/admin/markets`
  - `Signal` (or `Activity`) → Signals → `/admin/signals`
  - `Server` → System Health → `/admin/system`
  - `Users` → Users → `/admin/users` (placeholder, greyed out for now)
- Bottom: A `← Back to App` link → `/dashboard`. `text-prism-text-3 hover:text-prism-text-1`.

---

## 15. ADMIN: Admin Dashboard (`/admin`)

**File**: `src/pages/admin/AdminDashboard.tsx`

### KPI Grid

`grid grid-cols-2 md:grid-cols-4 gap-4 mt-6`.

Each KPI card:
```
┌───────────────────┐
│  TOTAL MARKETS    │  ← font-mono text-[10px] uppercase text-prism-text-3
│                   │
│  142              │  ← font-mono text-3xl font-bold text-prism-text-1
│  +3 today         │  ← font-sans text-xs text-signal-green (if positive)
└───────────────────┘
```
Border: `border border-prism-navy-3 bg-prism-navy-2 rounded-lg p-4`.

KPIs:
1. Total Markets tracked.
2. Signals in last 24h.
3. AI Insight calls (Gemini).
4. DB Snapshot count.

### System Status Row

`flex flex-wrap gap-3 mt-6`.

Each service status pill:
```
[● green] Bayse API      CONNECTED
[● green] PostgreSQL     HEALTHY
[● green] Redis          CONNECTED
[● amber] Gemini API     DEGRADED (3 failures/hr)
```
Style: `flex items-center gap-2 bg-prism-navy-2 border border-prism-navy-3 rounded-full px-3 py-1.5 font-mono text-xs text-prism-text-2`.
Status dot: 8px circle, `rounded-full`, colored by state.

### Recent Signal Activity Chart

A D3 bar chart: signals per hour for the last 24 hours, stacked by classification (green/amber/red).
Width: full. Height: 160px. Same chart styling as elsewhere.

### Recent Admin Log

A table of the last 10 background job runs:
`TIMESTAMP | JOB | STATUS | DURATION | NOTE`
Monospaced, `text-xs`. Status colored: `SUCCESS` green, `FAILED` red, `RUNNING` amber.

---

## 16. ADMIN: Admin Markets (`/admin/markets`)

**File**: `src/pages/admin/AdminMarkets.tsx`

### Page Header + Search

Title: `Market Monitor`.
Sub: `All Bayse markets currently tracked by Prism.`

**Search bar**: Full-width on mobile, 300px on desktop. `bg-prism-navy-2 border border-prism-steel rounded-md px-3 py-2 font-mono text-sm text-prism-text-1 placeholder:text-prism-text-3 focus:border-prism-accent`. Searches by market title (client-side, TanStack Query data).

**Sort + Filter controls** (same chip pattern as dashboard filter bar):
Filter by category: chips for `POLITICS | SPORTS | ENTERTAINMENT | CRYPTO | ALL`.

### Markets Table

Full-width, sortable by clicking column headers.

**Columns**: `MARKET ID | TITLE | CATEGORY | LIQUIDITY | ORDERS | LATEST PROBABILITY | LAST SNAPSHOT | SIGNALS DETECTED | ACTIONS`

Each row:
- Market ID: `font-mono text-xs text-prism-text-3` (truncated to 12 chars).
- Title: `font-sans text-sm text-prism-text-1`.
- Category: classification chip style but category-colored.
- Liquidity: `font-mono text-xs text-prism-text-mono`.
- Latest probability: colored bar + percentage. Bar: `bg-prism-navy-3 rounded-full h-1 w-20` with fill = percentage.
- Last snapshot: relative time.
- Signals detected: count badge.
- Actions: `View` icon button → `/market/:id`.

**Pagination**: 20 per page.

**Row hover**: `hover:bg-prism-navy-2`.

---

## 17. ADMIN: Admin Signals (`/admin/signals`)

**File**: `src/pages/admin/AdminSignals.tsx`

### Page Header + Controls

Title: `Signal Log`.
Sub: `All signals detected and scored by Prism.`

**Controls row** (`flex flex-wrap gap-3 items-center mt-4`):
- Classification filter chips.
- Score range slider (optional — two-thumb range, min 0 max 100). Built with a custom CSS range component (no external library). Style: track `bg-prism-navy-3`, fill `bg-prism-accent`, thumb `bg-prism-text-1 border-2 border-prism-accent rounded-full`.
- Date range picker — two date inputs, `type="date"`, same input style as login form.

### Signals Table

**Columns**: `ID | MARKET | SCORE | CLASS | ΔPROB | LIQUIDITY | VOL RATIO | ORDERS | AI? | TIME | OUTCOME`

`AI?` column: A small `Sparkles` Lucide icon in `text-prism-accent` if an AI insight exists, else `—`.
`OUTCOME` column: Chip if resolved, `—` if pending.

**Expandable rows**: Click a row → below it, a full-width expanded panel slides down (GSAP `height` tween from 0 to auto, 0.2s):
```
┌─────────────────────────────────────────────────────┐
│  SIGNAL DETAIL  #1042                               │
│                                                     │
│  Scoring breakdown:                                 │
│  MoveFactor:      0.87  ██████████░░             │
│  LiquidityFactor: 0.72  ████████░░░░             │
│  VolumeFactor:    0.95  ███████████░             │
│  OrderFactor:     0.61  ████████░░░░             │
│                                                     │
│  AI Insight:                                        │
│  "Strong participation..."                          │
└─────────────────────────────────────────────────────┘
```
Factor bars: `bg-prism-navy-4 h-1.5 rounded-full` with fill in `bg-prism-accent` proportional to factor value.

**Export button**: Top right. `↓ Export CSV` — calls `GET /admin/signals/export`. Style: `border border-prism-steel text-prism-text-2 hover:text-prism-text-1 text-xs font-mono px-3 py-1.5 rounded-md`.

---

## 18. ADMIN: Admin System Health (`/admin/system`)

**File**: `src/pages/admin/AdminSystem.tsx`

### Service Status Cards

`grid grid-cols-1 md:grid-cols-2 gap-4 mt-6`.

Each card (`bg-prism-navy-2 border rounded-lg p-5`):
Border color: `border-signal-green-muted` if healthy, `border-signal-red-muted` if down, `border-signal-amber-muted` if degraded.

```
┌──────────────────────────────┐
│  [● HEALTHY]   POSTGRESQL    │
│  ─────────────────────────   │
│  Response time:  4ms         │
│  Connections:    3/20        │
│  Last check:     12s ago     │
└──────────────────────────────┘
```

Services: PostgreSQL, Redis, Bayse API, Gemini API, APScheduler (background jobs).

### Job Schedule Panel

Table of all background jobs:
```
JOB NAME          │  FREQUENCY  │  LAST RUN    │  STATUS   │  NEXT RUN
ingestion_job     │  60s        │  5s ago      │  SUCCESS  │  55s
signal_job        │  60s        │  5s ago      │  SUCCESS  │  55s
outcome_job       │  30min      │  12min ago   │  SUCCESS  │  18min
```
Font: `font-mono text-xs`. Status colored. `NEXT RUN` shows a countdown that ticks down in real time (via `setInterval`, updating every second).

### Error Log

The last 20 errors captured by the system. Columns: `TIMESTAMP | SERVICE | ERROR | COUNT`.
Collapsible: clicking shows full stack trace in a code block.

---

## 19. Component Library Reference

> These are the shared components used across all pages. Define them precisely so the agent builds them once and reuses everywhere.

### 19.1 `ScoreBadge` (`src/components/ui/ScoreBadge.tsx`)

**Props**: `score: number, size?: 'sm' | 'md' | 'lg'`

**Sizes**:
- `sm`: 32×32px, font-size 11px.
- `md`: 44×44px, font-size 14px (default).
- `lg`: 64×64px, font-size 20px.

**Visual**: A circle. Background and border color based on classification:
- ≥70: `bg-signal-green-bg border-2 border-signal-green text-signal-green`.
- 40–69: `bg-signal-amber-bg border-2 border-signal-amber text-signal-amber`.
- <40: `bg-signal-red-bg border-2 border-signal-red text-signal-red`.

Font: `font-mono font-bold`. Number shown as integer (Math.round).

**Animation**: On first render, the number counts up from 0 → score over 0.6s using GSAP `fromTo` on a local state value. Triggered by `useEffect` on mount.

### 19.2 `ClassificationChip` (`src/components/ui/ClassificationChip.tsx`)

**Props**: `classification: 'INFORMED_MOVE' | 'UNCERTAIN' | 'NOISE'`

A pill-shaped chip. Width auto.

- `INFORMED_MOVE`: `bg-signal-green-bg border border-signal-green-muted text-signal-green`.
- `UNCERTAIN`: amber variant.
- `NOISE`: red variant.

Display text: `INFORMED MOVE` / `UNCERTAIN` / `NOISE`.
Style: `font-mono text-[10px] uppercase tracking-wide px-2.5 py-1 rounded-full border`.

### 19.3 `SignalCard` (`src/components/ui/SignalCard.tsx`)

**Props**: `signal: Signal` (from `src/types/signal.ts`), `mode?: 'full' | 'preview'`

**Full card structure** (`bg-prism-navy-2 border border-prism-navy-3 hover:border-prism-steel rounded-lg p-4 transition-all cursor-pointer`):

```
┌────────────────────────────────────────────────────────┐
│  ROW 1: [ScoreBadge md]  [Market Title]  [time ago]   │
│         ← left           ← flex-1, truncate           │
│                                                        │
│  ROW 2: [ClassificationChip]  [0.52 → 0.64 (+12%)]   │
│                                 font-mono text-sm      │
│                                                        │
│  ROW 3: NGN 2.1M liquidity · 842 orders · 3.2x vol   │
│         font-mono text-xs text-prism-text-3           │
│                                                        │
│  ROW 4 (if AI insight exists):                        │
│  "Strong participation and deep liquidity suggest..." │
│  font-sans text-xs italic text-prism-text-2           │
│  border-t border-prism-navy-3/50 pt-3 mt-3            │
│                                                        │
│  ROW 5: [View Market →]     ← right-aligned           │
│         font-sans text-xs text-prism-accent           │
└────────────────────────────────────────────────────────┘
```

**Left border accent** (the key visual differentiator): A 3px left border in the classification color:
- INFORMED: `border-l-[3px] border-l-signal-green`.
- UNCERTAIN: `border-l-[3px] border-l-signal-amber`.
- NOISE: `border-l-[3px] border-l-signal-red`.
Override the default `border-prism-navy-3` on the left side. Total: `border border-prism-navy-3 border-l-[3px] border-l-signal-green`.

**Hover state**: `hover:bg-prism-navy-3 hover:shadow-[0_2px_16px_rgba(0,0,0,0.3)]`.

**GSAP on card mount**: Slide in from `opacity: 0, y: 16` → `opacity: 1, y: 0`, 0.3s. Stagger by 60ms per card in the feed using `gsap.from('.signal-card', { stagger: 0.06 })`.

**On click**: Navigate to `/market/:id`.

**Preview mode** (landing page): Hide ROW 5 (View Market). Blur ROW 4 AI insight if not authenticated.

### 19.4 `SignalCardSkeleton` (`src/components/ui/SignalCardSkeleton.tsx`)

Same dimensions as `SignalCard`. All content replaced with `bg-prism-navy-3 rounded animate-pulse` blocks:
- Score circle: 44px circle.
- Title: `h-4 w-48 rounded`.
- Classification chip: `h-5 w-24 rounded-full`.
- Data row: `h-3 w-64 rounded`.
- AI text: `h-3 w-full rounded`, `h-3 w-3/4 rounded`.

### 19.5 `Toast` (`src/components/ui/Toast.tsx`)

Global toast notification system. Positioned `fixed bottom-4 right-4 z-[100]`.
Uses a `toasts` state array in a React Context (`ToastContext`).
Each toast: `bg-prism-navy-2 border border-prism-steel rounded-lg px-4 py-3 flex items-center gap-3 shadow-[0_4px_24px_rgba(0,0,0,0.5)]`.
Icons: `CheckCircle` (success, green), `AlertCircle` (error, red), `Info` (info, blue).
Text: `font-sans text-sm text-prism-text-1`.
Auto-dismiss after 4s. GSAP slide-in from `x: 40, opacity: 0` → `x: 0, opacity: 1`, and slide-out when dismissing.

### 19.6 `DataRow` (`src/components/ui/DataRow.tsx`)

**Props**: `label: string, value: string | number, mono?: boolean`
A single label/value row used in detail views:
```
Liquidity    NGN 2,100,000
```
`flex justify-between items-center py-1.5 border-b border-prism-navy-3/30`.
Label: `font-sans text-xs text-prism-text-3`.
Value: `font-mono text-xs text-prism-text-mono` (if mono), else `font-sans text-xs text-prism-text-2`.

### 19.7 `EmptyState` (`src/components/ui/EmptyState.tsx`)

**Props**: `icon: LucideIcon, title: string, description: string`
Centered flex column, `py-16`.
Icon: 32px, `text-prism-text-3`.
Title: `font-sans text-sm font-medium text-prism-text-2 mt-3`.
Description: `font-sans text-xs text-prism-text-3 mt-1 max-w-xs text-center`.

### 19.8 `PageTitle` (`src/components/ui/PageTitle.tsx`)

**Props**: `title: string, subtitle?: string`
`mb-6`.
Title: `font-display text-xl font-bold text-prism-text-1`.
Subtitle: `font-sans text-sm text-prism-text-3 mt-1`.

---

## 20. Animation & GSAP Directives

### Global Scroll Animations

Use GSAP ScrollTrigger on all landing page sections. Register plugin on app init:
```js
// In main.tsx or App.tsx
import { gsap } from 'gsap';
import { ScrollTrigger } from 'gsap/ScrollTrigger';
gsap.registerPlugin(ScrollTrigger);
```

**Standard entry animation** (use across all landing sections):
```js
gsap.fromTo('.section-enter', 
  { opacity: 0, y: 32 },
  { opacity: 1, y: 0, duration: 0.6, ease: 'power2.out',
    scrollTrigger: { trigger: '.section-enter', start: 'top 80%' }
  }
);
```
Apply className `section-enter` to any element that should fade in on scroll.

### Dashboard Feed Updates

When TanStack Query delivers fresh data and new signals are added to the feed:
```js
// Animate only newly prepended cards
gsap.from(newCardRefs, { opacity: 0, y: -16, duration: 0.3, stagger: 0.05, ease: 'power2.out' });
```

### Score Badge Count-Up

```js
// In ScoreBadge useEffect
gsap.fromTo(counterRef, { textContent: 0 }, {
  textContent: score,
  duration: 0.6,
  ease: 'power2.out',
  snap: { textContent: 1 },
  onUpdate: function() {
    counterRef.current.textContent = Math.round(this.targets()[0].textContent);
  }
});
```

### Page Transitions

Use GSAP `opacity` + `y` for page enter/exit. Wrap in a `PageTransition` component:
- Enter: `opacity: 0, y: 8` → `opacity: 1, y: 0`, 0.25s.
- Exit: `opacity: 1` → `opacity: 0`, 0.15s.
Trigger on route change via TanStack Router's `onBeforeLoad` / `onLoad`.

### Hover Micro-interactions

All interactive cards: add `transition-all duration-150` via Tailwind. Slight `translateY` on hover is done via Tailwind `hover:-translate-y-0.5` (2px lift). No heavy GSAP needed for simple hover states.

### Number Formatting

Use `Intl.NumberFormat` for all numbers:
- NGN amounts: `Intl.NumberFormat('en-NG', { style: 'currency', currency: 'NGN', notation: 'compact' })` → `NGN 2.1M`.
- Probabilities: `(prob * 100).toFixed(1) + '%'`.
- Timestamps: use `date-fns` `formatDistanceToNow()` → `2 min ago`.

---

## 21. Routing & Navigation Flow Map

### TanStack Router Route Tree

```
/                     Landing (public)
/login                Login (public, redirect if authed)
/register             Register (public, redirect if authed)
/dashboard            Dashboard (auth required)
/market/:id           Market Detail (auth required)
/performance          Performance Tracker (auth required)
/methodology          Methodology (public)
/admin                Admin Dashboard (admin role required)
/admin/markets        Admin Markets (admin role required)
/admin/signals        Admin Signals (admin role required)
/admin/system         Admin System (admin role required)
```

### Auth Guard Pattern

```tsx
// src/router.tsx
const authGuard = async ({ context }) => {
  const { user } = context;
  if (!user) throw redirect({ to: '/login' });
};
const adminGuard = async ({ context }) => {
  const { user } = context;
  if (!user) throw redirect({ to: '/login' });
  if (user.role !== 'admin') throw redirect({ to: '/dashboard' });
};
```

### Button → Navigation Map

| Element | Location | Action |
|---|---|---|
| `Get Access` CTA | Landing hero | Navigate to `/register` |
| `View Live Signals` | Landing hero | Navigate to `/dashboard` (or `/login` if not authed) |
| `Sign In` nav | Landing navbar | Navigate to `/login` |
| `Methodology` nav | Landing navbar | Navigate to `/methodology` |
| `Create one` | Login page | Navigate to `/register` |
| `Sign in` | Register page | Navigate to `/login` |
| `View Market →` | SignalCard | Navigate to `/market/:id` |
| `← Back to Feed` | Market Detail | Navigate to `/dashboard` |
| Dashboard nav item | Sidebar | Navigate to `/dashboard` |
| Performance nav item | Sidebar | Navigate to `/performance` |
| Methodology nav item | Sidebar | Navigate to `/methodology` |
| ADMIN chip | Header | Navigate to `/admin` |
| Admin nav items | Admin sidebar | Navigate to respective admin routes |
| `← Back to App` | Admin sidebar | Navigate to `/dashboard` |
| `Sign Out` | Avatar dropdown | POST `/auth/logout` → navigate to `/login` |
| Signal row expand | Admin Signals table | Expand in-place (no navigation) |
| Market row `View` | Admin Markets table | Navigate to `/market/:id` |

---

## 22. Cookie-Based Auth Architecture

> The frontend must NEVER use localStorage or sessionStorage for auth tokens. All auth state is managed via HTTP-only cookies set by the backend.

### Session Management Pattern

```tsx
// src/context/AuthContext.tsx
interface AuthContextType {
  user: User | null;
  isLoading: boolean;
  login: (email: string, password: string) => Promise<void>;
  logout: () => Promise<void>;
}
```

On **app init** (`App.tsx`):
1. Call `GET /auth/me` (credentials: `'include'` to send cookie).
2. If 200: set user in context.
3. If 401: set user to null, redirect protected routes to `/login`.
4. Loading state: show `SplashLoader` until `/auth/me` resolves.

On **login**:
1. POST `/auth/login` with `credentials: 'include'`.
2. Server sets `Set-Cookie: session=...; HttpOnly; Secure; SameSite=Strict`.
3. Store user object (from response body) in React Context only.
4. Navigate to `/dashboard`.

On **logout**:
1. POST `/auth/logout` with `credentials: 'include'`.
2. Server clears the cookie.
3. Clear user from React Context.
4. Navigate to `/login`.

### TanStack Query Auth Config

```tsx
// All API calls use:
const api = axios.create({
  baseURL: import.meta.env.VITE_API_URL,
  withCredentials: true, // sends cookies on every request
});

// Global error interceptor
api.interceptors.response.use(null, (error) => {
  if (error.response?.status === 401) {
    // Clear auth context + redirect to /login
    authContext.setUser(null);
    router.navigate({ to: '/login' });
  }
  return Promise.reject(error);
});
```

---

## 23. Responsive Breakpoint Rules

| Breakpoint | Value | Key Layout Changes |
|---|---|---|
| Default (mobile) | 0–767px | Single column. Sidebar hidden. Bottom nav shown. Stats strip horizontal scroll. Dashboard feed full width. |
| `md` | 768px+ | Sidebar shown (240px). Bottom nav hidden. Dashboard two-column (feed + stats panel). Landing two-column comparisons. |
| `lg` | 1024px+ | Landing hero text scales up. Admin tables show all columns. |
| `xl` | 1280px+ | Max-width containers apply. Page content centered. |

### Mobile-specific rules:
- All `font-mono` numbers scale down one size.
- SignalCard: ROW 3 (data row) wraps onto two lines.
- D3 charts: height reduces by 40px, some axis labels hidden via `tickValues` reduction.
- Admin tables: hide secondary columns (`ORDERS`, `VOL RATIO`) on mobile. Show core 5 columns only.
- Landing Three.js canvas: particle count 200 (from 600).
- Landing hero text: `text-4xl` (from `text-7xl`).

---

## 24. Empty, Loading & Error States

### Loading States

**Initial app load**: `SplashLoader` component (full screen).
**Page data loading**: Skeleton screens using `SignalCardSkeleton` and custom `div` skeletons with `animate-pulse bg-prism-navy-3 rounded`.
**Refetching in background**: Subtle spinner in the header live indicator area, no full-page overlay.
**Button loading**: Spinner icon (`Loader2`) replaces button icon, button disabled, opacity 70%.

### Error States

**Network error** (API unreachable): Full-width banner at top of page:
```
⚠  Unable to reach Prism API. Showing cached data.
```
`bg-signal-amber-bg border-b border-signal-amber-muted text-signal-amber font-mono text-xs py-2 px-4 text-center`.

**Empty feed** (no signals yet):
`EmptyState` component with `Activity` icon and text: `Monitoring Bayse markets — signals will appear here when detected.`

**404** (`/404` route):
```
404
Signal not found.
```
`font-mono text-[80px] font-bold text-prism-navy-3`. Below: `This page doesn't exist.` in `font-sans text-sm text-prism-text-3`. Link back to `/dashboard`.

**Auth error** (session expired): Redirected to `/login` with a toast: `Your session has expired. Please sign in again.` (amber toast).

---

## APPENDIX A: File Structure Reference

```
src/
├── main.tsx
├── App.tsx
├── router.tsx
├── context/
│   ├── AuthContext.tsx
│   └── ToastContext.tsx
├── lib/
│   ├── api.ts
│   └── utils.ts
├── types/
│   ├── market.ts
│   └── signal.ts
├── hooks/
│   ├── useSignals.ts
│   ├── useMarkets.ts
│   └── usePerformance.ts
├── components/
│   ├── layout/
│   │   ├── Header.tsx
│   │   ├── Sidebar.tsx
│   │   ├── BottomNav.tsx
│   │   └── Layout.tsx
│   ├── admin/
│   │   ├── AdminHeader.tsx
│   │   └── AdminSidebar.tsx
│   ├── ui/
│   │   ├── Logo.tsx
│   │   ├── SplashLoader.tsx
│   │   ├── ScoreBadge.tsx
│   │   ├── ClassificationChip.tsx
│   │   ├── SignalCard.tsx
│   │   ├── SignalCardSkeleton.tsx
│   │   ├── Toast.tsx
│   │   ├── DataRow.tsx
│   │   ├── EmptyState.tsx
│   │   └── PageTitle.tsx
│   ├── charts/
│   │   ├── ProbabilityChart.tsx
│   │   ├── ScoreDistribution.tsx
│   │   └── AccuracyChart.tsx
│   └── landing/
│       ├── HeroCanvas.tsx
│       └── ScoreFlowDiagram.tsx
├── pages/
│   ├── Landing.tsx
│   ├── Login.tsx
│   ├── Register.tsx
│   ├── Dashboard.tsx
│   ├── MarketDetail.tsx
│   ├── Performance.tsx
│   ├── Methodology.tsx
│   └── admin/
│       ├── AdminDashboard.tsx
│       ├── AdminMarkets.tsx
│       ├── AdminSignals.tsx
│       └── AdminSystem.tsx
└── styles/
    └── globals.css
```

---

## APPENDIX B: Dependencies to Install

```bash
# Core
npm install react react-dom
npm install @tanstack/react-query @tanstack/react-router
npm install axios
npm install gsap
npm install d3
npm install three
npm install lucide-react
npm install date-fns
npm install tailwindcss postcss autoprefixer
npm install @types/d3 @types/three

# Dev
npm install -D vite @vitejs/plugin-react typescript
```

---

## APPENDIX C: CSS Globals

```css
/* src/styles/globals.css */

:root {
  color-scheme: dark;
}

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

html {
  scroll-behavior: smooth;
}

body {
  background-color: #050D1A;
  color: #E2E8F0;
  font-family: 'Inter', system-ui, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

/* Custom scrollbar — webkit */
::-webkit-scrollbar { width: 6px; height: 6px; }
::-webkit-scrollbar-track { background: #0D1B2E; }
::-webkit-scrollbar-thumb { background: #2D4A6B; border-radius: 3px; }
::-webkit-scrollbar-thumb:hover { background: #3B5A7A; }

/* Selection */
::selection { background: rgba(59, 130, 246, 0.3); color: #E2E8F0; }

/* Remove tap highlight on mobile */
* { -webkit-tap-highlight-color: transparent; }

/* D3 charts — prevent SVG overflow */
svg { overflow: visible; }
```

---

*End of Prism Frontend UI/UX Specification v1.0*
*This document is the single source of truth for the Prism frontend build.*
*Every component, every color, every animation, and every navigation flow is defined above.*