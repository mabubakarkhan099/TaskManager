---
name: ui-ux-pro-max
description: "UI/UX design intelligence for web, mobile, and desktop. Use when designing, building, reviewing, or fixing interfaces: pages, components, design systems, accessibility, interaction, responsive layout, typography, color, charts, and stack-specific UI. Self-contained: includes a 10-category prioritized rule set (accessibility → charts), a product-type → style/palette/landing-pattern lookup (192 product types), 50 UI styles, 74 font pairings, 34 landing patterns, 25 chart types, GSAP motion presets, and a pre-delivery checklist."
---

# UI/UX Pro Max — Design Intelligence (Standalone)

A self-contained knowledge base for making interface design decisions: what style fits a product, which palette and fonts to use, how to structure a page, and which usability/accessibility rules to enforce. Everything is in this file, so no scripts, Python or data files are required.

> Derived from the open-source repo `nextlevelbuilder/ui-ux-pro-max-skill` (MIT License, © Next Level Builder). The original ships a Python search CLI over CSV datasets; this file inlines the condensed data and the full rule text so it works in any environment, including claude.ai. If you do have the repo checked out (e.g. in Claude Code), its `search.py` can be used for deeper lookups; see "Optional: the original search CLI" at the end.

## When to Apply

Use this skill when the task involves **UI structure, visual design decisions, interaction patterns, or UX quality control**: designing new pages, creating or refactoring components, choosing color/typography/spacing/layout systems, reviewing UI for UX/accessibility/consistency, implementing navigation/animation/responsive behavior, or improving perceived quality and usability.

Skip it for pure backend logic, API/database design, non-visual performance work, infrastructure/DevOps, or non-visual scripts, unless the task changes how something **looks, feels, moves, or is interacted with**.

## Rule Categories by Priority

Follow priority 1→10 to decide which category to focus on first. Full rule text for every category is in **Part 1** below.

| Priority | Category | Impact | Key Checks (Must Have) | Anti-Patterns (Avoid) |
|----------|----------|--------|------------------------|------------------------|
| 1 | Accessibility | CRITICAL | Contrast 4.5:1, alt text, keyboard nav, aria-labels | Removing focus rings, icon-only buttons without labels |
| 2 | Touch & Interaction | CRITICAL | Min size 44×44px, 8px+ spacing, loading feedback | Reliance on hover only, instant state changes (0ms) |
| 3 | Performance | HIGH | WebP/AVIF, lazy loading, reserve space (CLS < 0.1) | Layout thrashing, cumulative layout shift |
| 4 | Style Selection | HIGH | Match product type, consistency, SVG icons (no emoji) | Mixing flat & skeuomorphic randomly, emoji as icons |
| 5 | Layout & Responsive | HIGH | Mobile-first breakpoints, viewport meta, no horizontal scroll | Horizontal scroll, fixed px container widths, disabling zoom |
| 6 | Typography & Color | MEDIUM | Base 16px, line-height 1.5, semantic color tokens | Body text < 12px, gray-on-gray, raw hex in components |
| 7 | Animation | MEDIUM | Context-aware timing, motion conveys meaning, spatial continuity | One duration for every transition, animating width/height, no reduced-motion |
| 8 | Forms & Feedback | MEDIUM | Visible labels, error near field, helper text, progressive disclosure | Placeholder-only label, errors only at top, overwhelming upfront |
| 9 | Navigation Patterns | HIGH | Predictable back, bottom nav ≤5, deep linking | Overloaded nav, broken back behavior, no deep links |
| 10 | Charts & Data | LOW | Legends, tooltips, accessible colors | Relying on color alone to convey meaning |

---

## Workflow

### Step 1: Analyze the request
Extract:
- **Product type**: SaaS, e-commerce, portfolio, dashboard, entertainment, tool, productivity, or hybrid
- **Audience & context**: age group, usage context (commute, leisure, work)
- **Style keywords**: playful, vibrant, minimal, dark mode, content-first, immersive, etc.
- **Stack**: detect from the project (`package.json` deps for react/next/vue/svelte/nuxt/@angular, `pubspec.yaml` for Flutter, `*.xcodeproj`/`Package.swift` for SwiftUI, `composer.json` for Laravel, `app.json` + `react-native` for React Native). If nothing is detectable and stack matters, ask the user. **Never assume a stack.**

### Step 2: Build a design system (REQUIRED for new pages/projects)
1. Find the closest row in **Part 2A (Product → Design Direction)**. It gives the primary style, secondary styles, landing pattern, dashboard style (if relevant) and a starting palette (`primary / accent / background`).
2. Look up the recommended style in **Part 2B (Styles)** for effects, best-for and do-not-use constraints. If the style is listed under "do not use for" something in the request, pick a secondary style instead.
3. Pick fonts from **Part 2C (Font Pairings)** matching the mood and product.
4. Pick the page structure from **Part 2D (Landing Patterns)**; pick chart types from **Part 2E** if data is shown.
5. Express the result as **semantic tokens** (see output template below) instead of raw hex in components.
6. If no row fits well, say so and use the closest general row (e.g. "SaaS (General)"), labeling it as a fallback rather than a database match.

### Step 3: Apply the rule set
Walk the priority table 1→10. For each category relevant to the task, apply the matching rules from **Part 1**. For reviews/audits, scan Part 1 in full.

### Step 4: Stack-specific implementation
Use the detected stack from Step 1 and follow its idioms (see **Part 3**). Prefer native/system controls and the stack's own component library over custom rebuilds.

### Step 5: Pre-delivery check
Run the **Pre-Delivery Checklist** in Part 4 before delivering. Report anything you could not verify.

### Design dials (optional)
If the user gives (or implies) any of these 1–10 sliders, bias the choices accordingly:

| Dial | Low (1–3) | Mid (4–7) | High (8–10) |
|------|-----------|-----------|-------------|
| Variance | Centered / minimal (lean Minimalism & Swiss) | Balanced / modern | Bold / asymmetric (lean Brutalism, Bento Grids) |
| Motion | Subtle micro-interactions | Standard scroll/stagger motion | Complex choreography (pin, Flip, SplitText) — see Part 2F |
| Density | Spacious (24–96px spacing scale) | Standard (16–64px) | Dense / dashboard (8–32px) |

### Output template for a design system
When asked to produce or summarize a design system, use:

```
Project: <name>
Product type: <row from Part 2A>      Stack: <detected>
Pattern: <landing/page pattern>       Style: <primary> (+ <secondary>)
Colors: primary #…, on-primary #…, secondary #…, accent #…, background #…, foreground #…,
        card #…, muted #…, muted-foreground #…, border #…, destructive #…, ring #…
Typography: <heading font> / <body font>   (scale: 12 14 16 18 24 32; body 16px, line-height 1.5–1.75)
Spacing: 4/8 rhythm; section tiers 16/24/32/48; container max-w-6xl/7xl
Effects: <from style row>; motion tokens (enter / exit ≈ 60–70% of enter / stagger 30–50ms)
Avoid: <anti-patterns from style row + rules>
```
Persist it (e.g. as `design-system/MASTER.md`) only if the user asks and a filesystem is available; read any existing master file first and never overwrite prior decisions silently.

### Handling gaps honestly
If nothing in the tables fits the request, do not fabricate a "data match". Say the recommendation comes from general defaults (the priority table and Part 1), and name the closest row you adapted.

---

# Part 1 — Full Rule Set (10 categories)

Use for UI review/audit passes and for checking any category beyond the priority table.

#### 1. Accessibility (CRITICAL)

- `color-contrast` - Minimum 4.5:1 ratio for normal text (large text 3:1); Material Design
- `focus-states` - Visible focus rings on interactive elements (2–4px; Apple HIG, MD)
- `alt-text` - Descriptive alt text for meaningful images
- `aria-labels` - aria-label for icon-only buttons; accessibilityLabel in native (Apple HIG)
- `icon-context` - Semantics depend on use: decorative icons beside visible text are hidden from the accessibility tree; meaningful icons need a text alternative; icon controls need an accessible name and applicable state
- `keyboard-nav` - Tab order matches visual order; full keyboard support (Apple HIG)
- `form-labels` - Use label with for attribute
- `skip-links` - Skip to main content for keyboard users
- `heading-hierarchy` - Sequential h1→h6, no level skip
- `color-not-only` - Don't convey info by color alone (add icon/text)
- `dynamic-type` - Support system text scaling; avoid truncation as text grows (Apple Dynamic Type, MD)
- `reduced-motion` - Respect prefers-reduced-motion; reduce/disable animations when requested (Apple Reduced Motion API, MD)
- `voiceover-sr` - Meaningful accessibilityLabel/accessibilityHint; logical reading order for VoiceOver/screen readers (Apple HIG, MD)
- `escape-routes` - Provide cancel/back in modals and multi-step flows (Apple HIG)
- `keyboard-shortcuts` - Preserve system and a11y shortcuts; offer keyboard alternatives for drag-and-drop (Apple HIG)
- `focus-not-obscured` - Sticky UI, overlays, and banners must not hide the keyboard-focused control (WCAG 2.2 AA)
- `focus-not-obscured-enhanced` - Keep the entire focused component visible (WCAG 2.2 AAA)
- `focus-appearance` - Verify focus indicator area and 3:1 state contrast; visible focus alone is not enough (WCAG 2.2 AAA)
- `dragging-alternative` - Every author-controlled drag action needs a single-pointer and keyboard alternative (WCAG 2.2 AA)
- `web-target-size` - Web pointer targets need 24×24 CSS px or a documented exception; do not substitute native units (WCAG 2.2 AA)
- `consistent-help` - Repeated help mechanisms stay in the same relative order across a page set (WCAG 2.2 A)
- `redundant-entry` - Reuse information already supplied in the same process unless re-entry is essential (WCAG 2.2 A)
- `accessible-authentication` - Allow password managers and paste; provide a non-cognitive authentication path (WCAG 2.2 Minimum, AA). The Enhanced AAA criterion is not represented in the dataset
- `auto-rotation-controls` - Carousels and moving content need pause/stop controls and must stop on focus or reduced motion (WAI)
- `contextual-live-badge-updates` - Announce a changed count/status as a complete contextual phrase without moving focus; use one appropriate live/status region and atomic updates only when needed

#### 2. Touch & Interaction (CRITICAL)

- `touch-target-size` - Min 44×44pt (Apple) / 48×48dp (Material); extend hit area beyond visual bounds if needed
- `touch-spacing` - Minimum 8px/8dp gap between touch targets (Apple HIG, MD)
- `hover-vs-tap` - Use click/tap for primary interactions; don't rely on hover alone
- `loading-buttons` - Disable button during async operations; show spinner or progress
- `error-feedback` - Clear error messages near problem
- `cursor-pointer` - Add cursor-pointer to clickable elements (Web)
- `gesture-conflicts` - Avoid horizontal swipe on main content; prefer vertical scroll
- `tap-delay` - Use touch-action: manipulation to reduce 300ms delay (Web)
- `standard-gestures` - Use platform standard gestures consistently; don't redefine (e.g. swipe-back, pinch-zoom) (Apple HIG)
- `system-gestures` - Don't block system gestures (Control Center, back swipe, etc.) (Apple HIG)
- `press-feedback` - Visual feedback on press (ripple/highlight; MD state layers)
- `haptic-feedback` - Use haptic for confirmations and important actions; avoid overuse (Apple HIG)
- `gesture-alternative` - Don't rely on gesture-only interactions; always provide visible controls for critical actions
- `safe-area-awareness` - Keep primary touch targets away from notch, Dynamic Island, gesture bar and screen edges
- `no-precision-required` - Avoid requiring pixel-perfect taps on small icons or thin edges
- `swipe-clarity` - Swipe actions must show clear affordance or hint (chevron, label, tutorial)
- `drag-threshold` - Use a movement threshold before starting drag to avoid accidental drags

#### 3. Performance (HIGH)

- `image-optimization` - Use WebP/AVIF, responsive images (srcset/sizes), lazy load non-critical assets
- `image-dimension` - Declare width/height or use aspect-ratio to prevent layout shift (Core Web Vitals: CLS)
- `font-loading` - Use font-display: swap/optional to avoid invisible text (FOIT); reserve space to reduce layout shift (MD)
- `font-preload` - Preload only critical fonts; avoid overusing preload on every variant
- `critical-css` - Prioritize above-the-fold CSS (inline critical CSS or early-loaded stylesheet)
- `lazy-loading` - Lazy load non-hero components via dynamic import / route-level splitting
- `bundle-splitting` - Split code by route/feature (React Suspense / Next.js dynamic) to reduce initial load and TTI
- `third-party-scripts` - Load third-party scripts async/defer; audit and remove unnecessary ones (MD)
- `reduce-reflows` - Avoid frequent layout reads/writes; batch DOM reads then writes
- `content-jumping` - Reserve space for async content to avoid layout jumps (Core Web Vitals: CLS)
- `lazy-load-below-fold` - Use loading="lazy" for below-the-fold images and heavy media
- `virtualize-lists` - Virtualize lists with 50+ items to improve memory efficiency and scroll performance
- `main-thread-budget` - Keep per-frame work under ~16ms for 60fps; move heavy tasks off main thread (HIG, MD)
- `progressive-loading` - Use skeleton screens / shimmer instead of long blocking spinners for >1s operations (Apple HIG)
- `input-latency` - Keep input latency under ~100ms for taps/scrolls (Material responsiveness standard)
- `tap-feedback-speed` - Provide visual feedback within 100ms of tap (Apple HIG)
- `debounce-throttle` - Use debounce/throttle for high-frequency events (scroll, resize, input)
- `offline-support` - Provide offline state messaging and basic fallback (PWA / mobile)
- `network-fallback` - Offer degraded modes for slow networks (lower-res images, fewer animations)

#### 4. Style Selection (HIGH)

- `style-match` - Match style to product type (use Part 2A/2B to pick the style for the product type)
- `consistency` - Use same style across all pages
- `no-emoji-icons` - Use SVG icons (Heroicons, Lucide), not emojis
- `color-palette-from-product` - Choose palette from product/industry (see the palette column in Part 2A)
- `effects-match-style` - Shadows, blur, radius aligned with chosen style (glass / flat / clay etc.)
- `platform-adaptive` - Respect platform idioms (iOS HIG vs Material): navigation, controls, typography, motion
- `state-clarity` - Make hover/pressed/disabled states visually distinct while staying on-style (Material state layers)
- `elevation-consistent` - Use a consistent elevation/shadow scale for cards, sheets, modals; avoid random shadow values
- `dark-mode-pairing` - Design light/dark variants together to keep brand, contrast, and style consistent
- `icon-style-consistent` - Use one icon set/visual language (stroke width, corner radius) across the product
- `system-controls` - Prefer native/system controls over fully custom ones; only customize when branding requires it (Apple HIG)
- `blur-purpose` - Use blur to indicate background dismissal (modals, sheets), not as decoration (Apple HIG)
- `primary-action` - Each screen should have only one primary CTA; secondary actions visually subordinate (Apple HIG)

#### 5. Layout & Responsive (HIGH)

- `viewport-meta` - width=device-width initial-scale=1 (never disable zoom)
- `mobile-first` - Design mobile-first, then scale up to tablet and desktop
- `breakpoint-consistency` - Use systematic breakpoints (e.g. 375 / 768 / 1024 / 1440)
- `readable-font-size` - Minimum 16px body text on mobile (avoids iOS auto-zoom)
- `line-length-control` - Mobile 35–60 chars per line; desktop 60–75 chars
- `horizontal-scroll` - No horizontal scroll on mobile; ensure content fits viewport width
- `spacing-scale` - Use 4pt/8dp incremental spacing system (Material Design)
- `touch-density` - Keep component spacing comfortable for touch: not cramped, not causing mis-taps
- `container-width` - Consistent max-width on desktop (max-w-6xl / 7xl)
- `z-index-management` - Define layered z-index scale (e.g. 0 / 10 / 20 / 40 / 100 / 1000)
- `fixed-element-offset` - Fixed navbar/bottom bar must reserve safe padding for underlying content
- `scroll-behavior` - Avoid nested scroll regions that interfere with the main scroll experience
- `viewport-units` - Prefer min-h-dvh over 100vh on mobile
- `orientation-support` - Keep layout readable and operable in landscape mode
- `content-priority` - Show core content first on mobile; fold or hide secondary content
- `visual-hierarchy` - Establish hierarchy via size, spacing, contrast — not color alone
- `compact-label-overflow` - Choose badge, status tag, filter chip, or removable value from its semantics; keep essential labels available and disclose unavoidable truncation to pointer and keyboard users
- `chip-collection-reflow` - Wrap the collection before shrinking labels; make a `+n` overflow summary an operable disclosure instead of hiding values

#### 6. Typography & Color (MEDIUM)

- `line-height` - Use 1.5-1.75 for body text
- `line-length` - Limit to 65-75 characters per line
- `font-pairing` - Match heading/body font personalities
- `font-scale` - Consistent type scale (e.g. 12 14 16 18 24 32)
- `contrast-readability` - Darker text on light backgrounds (e.g. slate-900 on white)
- `text-styles-system` - Use platform type system: iOS 11 Dynamic Type styles / Material 5 type roles (display, headline, title, body, label) (HIG, MD)
- `weight-hierarchy` - Use font-weight to reinforce hierarchy: Bold headings (600–700), Regular body (400), Medium labels (500) (MD)
- `color-semantic` - Define semantic color tokens (primary, secondary, error, surface, on-surface) not raw hex in components (Material color system)
- `color-dark-mode` - Dark mode uses desaturated / lighter tonal variants, not inverted colors; test contrast separately (HIG, MD)
- `color-accessible-pairs` - Foreground/background pairs must meet 4.5:1 (AA) or 7:1 (AAA); use tools to verify (WCAG, MD)
- `color-not-decorative-only` - Functional color (error red, success green) must include icon/text; avoid color-only meaning (HIG, MD)
- `truncation-strategy` - Prefer wrapping over truncation; when truncating use ellipsis and provide full text via tooltip/expand (Apple HIG)
- `letter-spacing` - Respect default letter-spacing per platform; avoid tight tracking on body text (HIG, MD)
- `number-tabular` - Use tabular/monospaced figures for data columns, prices, and timers to prevent layout shift
- `whitespace-balance` - Use whitespace intentionally to group related items and separate sections; avoid visual clutter (Apple HIG)
- `heading-line-balance` - Use balanced wrapping on short headings as a progressive, user-agent-controlled heuristic; keep natural wrapping readable and never force final words together with blanket nonbreaking spaces
- `long-token-wrapping` - Let URLs, IDs, and user content reflow with `overflow-wrap: anywhere` and a shrinkable flex/grid text child; do not apply `word-break: break-all` to normal prose

#### 7. Animation (MEDIUM)

- `duration-timing` - Choose shared motion tokens by distance, complexity, platform, and user context; test that feedback remains responsive instead of treating one duration range as universal
- `transform-performance` - Use transform/opacity only; avoid animating width/height/top/left
- `loading-states` - Match feedback to the expected wait and platform/component guidance; avoid both flashing indicators for near-instant work and unexplained long waits
- `excessive-motion` - Animate 1-2 key elements per view max
- `easing` - Use deceleration when arriving, acceleration when leaving, and linear motion for genuinely constant-rate progress or rotation
- `motion-meaning` - Every animation must express a cause-effect relationship, not just be decorative (Apple HIG)
- `state-transition` - State changes (hover / active / expanded / collapsed / modal) should animate smoothly, not snap
- `continuity` - Page/screen transitions should maintain spatial continuity (shared element, directional slide) (Apple HIG)
- `parallax-subtle` - Use parallax sparingly; must respect reduced-motion and not cause disorientation (Apple HIG)
- `spring-physics` - Prefer spring/physics-based curves over linear or cubic-bezier for natural feel (Apple HIG fluid animations)
- `exit-faster-than-enter` - Exit animations shorter than enter (~60–70% of enter duration) to feel responsive (MD motion)
- `stagger-sequence` - Stagger list/grid item entrance by 30–50ms per item; avoid all-at-once or too-slow reveals (MD)
- `shared-element-transition` - Use shared element / hero transitions for visual continuity between screens (MD, HIG)
- `interruptible` - Animations must be interruptible; user tap/gesture cancels in-progress animation immediately (Apple HIG)
- `no-blocking-animation` - Never block user input during an animation; UI must stay interactive (Apple HIG)
- `fade-crossfade` - Use crossfade for content replacement within the same container (MD)
- `scale-feedback` - Subtle scale (0.95–1.05) on press for tappable cards/buttons; restore on release (HIG, MD)
- `gesture-feedback` - Drag, swipe, and pinch must provide real-time visual response tracking the finger (MD Motion)
- `hierarchy-motion` - Use translate/scale direction to express hierarchy: enter from below = deeper, exit upward = back (MD)
- `motion-consistency` - Unify duration/easing tokens globally; all animations share the same rhythm and feel
- `opacity-threshold` - Fading elements should not linger below opacity 0.2; either fade fully or remain visible
- `modal-motion` - Modals/sheets should animate from their trigger source (scale+fade or slide-in) for spatial context (HIG, MD)
- `navigation-direction` - Forward navigation animates left/up; backward animates right/down — keep direction logically consistent (HIG)
- `layout-shift-avoid` - Animations must not cause layout reflow or CLS; use transform for position changes
- `cancellable-state-transitions` - Rapid state changes must cancel/replace prior micro-interactions safely, set the new final state explicitly, and never depend on an animation-end event for correctness

#### 8. Forms & Feedback (MEDIUM)

- `input-labels` - Visible label per input (not placeholder-only)
- `error-placement` - Show a specific error below the related field and connect it with aria-describedby
- `submit-feedback` - Loading then success/error state on submit
- `required-indicators` - Mark required fields (e.g. asterisk)
- `empty-states` - Helpful message and action when no content
- `toast-dismiss` - Auto-dismiss toasts in 3-5s
- `confirmation-dialogs` - Confirm before destructive actions
- `input-helper-text` - Provide persistent helper text below complex inputs, not just placeholder (Material Design)
- `disabled-states` - Disabled elements use reduced opacity (0.38–0.5) + cursor change + semantic attribute (MD)
- `progressive-disclosure` - Reveal complex options progressively; don't overwhelm users upfront (Apple HIG)
- `inline-validation` - Validate on blur (not keystroke); show error only after user finishes input (MD)
- `input-type-keyboard` - Use semantic input types (email, tel, number) to trigger the correct mobile keyboard (HIG, MD)
- `password-toggle` - Provide show/hide toggle for password fields (MD)
- `autofill-support` - Use autocomplete / textContentType attributes so the system can autofill (HIG, MD)
- `undo-support` - Allow undo for destructive or bulk actions (e.g. "Undo delete" toast) (Apple HIG)
- `success-feedback` - Confirm completed actions with brief visual feedback (checkmark, toast, color flash) (MD)
- `error-recovery` - Error messages must include a clear recovery path (retry, edit, help link) (HIG, MD)
- `multi-step-progress` - Multi-step flows show step indicator or progress bar; allow back navigation (MD)
- `form-autosave` - Long forms should auto-save drafts to prevent data loss on accidental dismissal (Apple HIG)
- `sheet-dismiss-confirm` - Confirm before dismissing a sheet/modal with unsaved changes (Apple HIG)
- `error-clarity` - Error messages must state cause + how to fix (not just "Invalid input") (HIG, MD)
- `field-grouping` - Group related fields logically (fieldset/legend or visual grouping) (MD)
- `read-only-distinction` - Read-only state should be visually and semantically different from disabled (MD)
- `focus-management` - After failed submission with multiple errors, focus the error summary; without a summary, focus the first invalid field
- `error-summary` - Put a focusable summary at the top after failed submit, link each item to its invalid field, and retain inline field errors
- `touch-friendly-input` - Mobile input height ≥44px to meet touch target requirements (Apple HIG)
- `destructive-emphasis` - Destructive actions use semantic danger color (red) and are visually separated from primary actions (HIG, MD)
- `toast-accessibility` - Toasts must not steal focus; use aria-live="polite" for screen reader announcement (WCAG)
- `aria-live-errors` - Form errors use aria-live region or role="alert" to notify screen readers (WCAG)
- `contrast-feedback` - Error and success state colors must meet 4.5:1 contrast ratio (WCAG, MD)
- `timeout-feedback` - Request timeout must show clear feedback with retry option (MD)

#### 9. Navigation Patterns (HIGH)

- `bottom-nav-limit` - Bottom navigation max 5 items; use labels with icons (Material Design)
- `drawer-usage` - Use drawer/sidebar for secondary navigation, not primary actions (Material Design)
- `back-behavior` - Back navigation must be predictable and consistent; preserve scroll/state (Apple HIG, MD)
- `deep-linking` - All key screens must be reachable via deep link / URL for sharing and notifications (Apple HIG, MD)
- `tab-bar-ios` - iOS: use bottom Tab Bar for top-level navigation (Apple HIG)
- `top-app-bar-android` - Android: use Top App Bar with navigation icon for primary structure (Material Design)
- `nav-label-icon` - Navigation items must have both icon and text label; icon-only nav harms discoverability (MD)
- `nav-state-active` - Current location must be visually highlighted (color, weight, indicator) in navigation (HIG, MD)
- `nav-hierarchy` - Primary nav (tabs/bottom bar) vs secondary nav (drawer/settings) must be clearly separated (MD)
- `modal-escape` - Modals and sheets must offer a clear close/dismiss affordance; swipe-down to dismiss on mobile (Apple HIG)
- `search-accessible` - Search must be easily reachable (top bar or tab); provide recent/suggested queries (MD)
- `breadcrumb-web` - Web: use breadcrumbs for 3+ level deep hierarchies to aid orientation (MD)
- `state-preservation` - Navigating back must restore previous scroll position, filter state, and input (HIG, MD)
- `gesture-nav-support` - Support system gesture navigation (iOS swipe-back, Android predictive back) without conflict (HIG, MD)
- `tab-badge` - Use badges on nav items sparingly to indicate unread/pending; clear after user visits (HIG, MD)
- `overflow-menu` - When actions exceed available space, use overflow/more menu instead of cramming (MD)
- `bottom-nav-top-level` - Bottom nav is for top-level screens only; never nest sub-navigation inside it (MD)
- `adaptive-navigation` - Large screens (≥1024px) prefer sidebar; small screens use bottom/top nav (Material Adaptive)
- `back-stack-integrity` - Never silently reset the navigation stack or unexpectedly jump to home (HIG, MD)
- `navigation-consistency` - Navigation placement must stay the same across all pages; don't change by page type
- `avoid-mixed-patterns` - Don't mix Tab + Sidebar + Bottom Nav at the same hierarchy level
- `modal-vs-navigation` - Modals must not be used for primary navigation flows; they break the user's path (HIG)
- `focus-on-route-change` - After page transition, move focus to main content region for screen reader users (WCAG)
- `persistent-nav` - Core navigation must remain reachable from deep pages; don't hide it entirely in sub-flows (HIG, MD)
- `destructive-nav-separation` - Dangerous actions (delete account, logout) must be visually and spatially separated from normal nav items (HIG, MD)
- `empty-nav-state` - When a nav destination is unavailable, explain why instead of silently hiding it (MD)

#### 10. Charts & Data (LOW)

- `chart-type` - Match chart type to data type (trend → line, comparison → bar, proportion → pie/donut)
- `color-guidance` - Use accessible color palettes; avoid red/green only pairs for colorblind users (WCAG, MD)
- `data-table` - Provide table alternative for accessibility; charts alone are not screen-reader friendly (WCAG)
- `pattern-texture` - Supplement color with patterns, textures, or shapes so data is distinguishable without color (WCAG, MD)
- `legend-visible` - Always show legend; position near the chart, not detached below a scroll fold (MD)
- `tooltip-on-interact` - Provide tooltips/data labels on hover (Web) or tap (mobile) showing exact values (HIG, MD)
- `axis-labels` - Label axes with units and readable scale; avoid truncated or rotated labels on mobile
- `responsive-chart` - Charts must reflow or simplify on small screens (e.g. horizontal bar instead of vertical, fewer ticks)
- `empty-data-state` - Show meaningful empty state when no data exists ("No data yet" + guidance), not a blank chart (MD)
- `loading-chart` - Use skeleton or shimmer placeholder while chart data loads; don't show an empty axis frame
- `animation-optional` - Chart entrance animations must respect prefers-reduced-motion; data should be readable immediately (HIG)
- `large-dataset` - For 1000+ data points, aggregate or sample; provide drill-down for detail instead of rendering all (MD)
- `number-formatting` - Use locale-aware formatting for numbers, dates, currencies on axes and labels (HIG, MD)
- `touch-target-chart` - Interactive chart elements (points, segments) must have ≥44pt tap area or expand on touch (Apple HIG)
- `no-pie-overuse` - Avoid pie/donut for >5 categories; switch to bar chart for clarity
- `contrast-data` - Data lines/bars vs background ≥3:1; data text labels ≥4.5:1 (WCAG)
- `legend-interactive` - Legends should be clickable to toggle series visibility (MD)
- `direct-labeling` - For small datasets, label values directly on the chart to reduce eye travel
- `tooltip-keyboard` - Tooltip content must be keyboard-reachable and not rely on hover alone (WCAG)
- `sortable-table` - Data tables must support sorting with aria-sort indicating current sort state (WCAG)
- `axis-readability` - Axis ticks must not be cramped; maintain readable spacing, auto-skip on small screens
- `data-density` - Limit information density per chart to avoid cognitive overload; split into multiple charts if needed
- `trend-emphasis` - Emphasize data trends over decoration; avoid heavy gradients/shadows that obscure the data
- `gridline-subtle` - Grid lines should be low-contrast (e.g. gray-200) so they don't compete with data
- `focusable-elements` - Interactive chart elements (points, bars, slices) must be keyboard-navigable (WCAG)
- `screen-reader-summary` - Provide a text summary or aria-label describing the chart's key insight for screen readers (WCAG)
- `error-state-chart` - Data load failure must show error message with retry action, not a broken/empty chart
- `export-option` - For data-heavy products, offer CSV/image export of chart data
- `drill-down-consistency` - Drill-down interactions must maintain a clear back-path and hierarchy breadcrumb
- `time-scale-clarity` - Time series charts must clearly label time granularity (day/week/month) and allow switching


---

# Part 2 — Design Knowledge Base

## 2A. Product → Design Direction (192 product types)
Columns: product type · primary style · landing pattern · dashboard style (if applicable) · starter palette `primary/accent/background` · palette focus. Palettes are starting points; always verify contrast (≥4.5:1 text, ≥3:1 UI) and adjust.

| Product type | Primary style | Landing pattern | Dashboard style | Palette (P/A/BG) | Palette focus |
|---|---|---|---|---|---|
| SaaS (General) | Glassmorphism + Flat Design | Hero + Features + CTA | Data-Dense + Real-Time Monitoring | #2563EB/#EA580C/#F8FAFC | Trust blue + accent contrast |
| Micro SaaS | Flat Design + Vibrant & Block-based | Minimal & Direct + Demo | Executive Dashboard | #6366F1/#059669/#F5F3FF | Vibrant primary + white space |
| E-commerce | Vibrant & Block-based | Feature-Rich Showcase | Sales Intelligence Dashboard | #059669/#EA580C/#ECFDF5 | Brand primary + success green |
| E-commerce Luxury | Liquid Glass + Glassmorphism | Feature-Rich Showcase | Sales Intelligence Dashboard | #1C1917/#A16207/#FAFAF9 | Premium colors + minimal accent |
| B2B Service | Accessible & Ethical + Minimalism & Swiss Style | Feature-Rich Showcase | Sales Intelligence Dashboard | #0F172A/#0369A1/#F8FAFC | Professional blue + neutral grey |
| Financial Dashboard | Dark Mode (OLED) + Data-Dense Dashboard | N/A - Dashboard focused | Financial Dashboard | #0F172A/#22C55E/#020617 | Dark bg + red/green alerts + trust blue |
| Analytics Dashboard | Data-Dense Dashboard + Heat Map & Heatmap Style | N/A - Analytics focused | Drill-Down Analytics + Comparative | #1E40AF/#D97706/#F8FAFC | Cool→Hot gradients + neutral grey |
| Healthcare App | Neumorphism + Accessible & Ethical | Social Proof-Focused | User Behavior Analytics | #0891B2/#059669/#ECFEFF | Calm blue + health green + trust |
| Educational App | Claymorphism + Micro-interactions | Storytelling-Driven | User Behavior Analytics | #4F46E5/#EA580C/#EEF2FF | Playful colors + clear hierarchy |
| Creative Agency | Brutalism + Motion-Driven | Storytelling-Driven | N/A - Portfolio focused | #EC4899/#0891B2/#FDF2F8 | Bold primaries + artistic freedom |
| Portfolio/Personal | Motion-Driven + Minimalism & Swiss Style | Storytelling-Driven | N/A - Personal branding | #18181B/#2563EB/#FAFAFA | Brand primary + artistic interpretation |
| Gaming | 3D & Hyperrealism + Retro-Futurism | Feature-Rich Showcase | N/A - Game focused | #7C3AED/#F43F5E/#0F0F23 | Vibrant + neon + immersive colors |
| Government/Public Service | Accessible & Ethical + Minimalism & Swiss Style | Minimal & Direct | Executive Dashboard | #0F172A/#0369A1/#F8FAFC | Professional blue + high contrast |
| Fintech/Crypto | Glassmorphism + Dark Mode (OLED) | Conversion-Optimized | Real-Time Monitoring + Predictive | #F59E0B/#8B5CF6/#0F172A | Dark tech colors + trust + vibrant accents |
| Social Media App | Vibrant & Block-based + Motion-Driven | Feature-Rich Showcase | User Behavior Analytics | #E11D48/#2563EB/#FFF1F2 | Vibrant + engagement colors |
| Productivity Tool | Flat Design + Micro-interactions | Interactive Product Demo | Drill-Down Analytics | #0D9488/#EA580C/#F0FDFA | Clear hierarchy + functional colors |
| Design System/Component Library | Minimalism & Swiss Style + Accessible & Ethical | Feature-Rich Showcase | N/A - Dev focused | #4F46E5/#EA580C/#EEF2FF | Clear hierarchy + code-like structure |
| AI/Chatbot Platform | AI-Native UI + Minimalism & Swiss Style | Interactive Product Demo | AI/ML Analytics Dashboard | #7C3AED/#0891B2/#FAF5FF | Neutral + AI Purple (#6366F1) |
| NFT/Web3 Platform | Cyberpunk UI + Glassmorphism | Feature-Rich Showcase | Crypto/Blockchain Dashboard | #8B5CF6/#FBBF24/#0F0F23 | Dark + Neon + Gold (#FFD700) |
| Creator Economy Platform | Vibrant & Block-based + Bento Box Grid | Social Proof-Focused | User Behavior Analytics | #EC4899/#EA580C/#FDF2F8 | Vibrant + Brand colors |
| Remote Work/Collaboration Tool | Soft UI Evolution + Minimalism & Swiss Style | Feature-Rich Showcase | Drill-Down Analytics | #6366F1/#059669/#F5F3FF | Calm Blue + Neutral grey |
| Mental Health App | Neumorphism + Accessible & Ethical | Social Proof-Focused | Healthcare Analytics | #8B5CF6/#059669/#FAF5FF | Calm Pastels + Trust colors |
| Pet Tech App | Claymorphism + Vibrant & Block-based | Storytelling-Driven | User Behavior Analytics | #F97316/#2563EB/#FFF7ED | Playful + Warm colors |
| Smart Home/IoT Dashboard | Glassmorphism + Dark Mode (OLED) | Interactive Product Demo | Real-Time Monitoring | #1E293B/#22C55E/#0F172A | Dark + Status indicator colors |
| EV/Charging Ecosystem | Minimalism & Swiss Style + Aurora UI | Hero-Centric Design | Energy/Utilities Dashboard | #0891B2/#16A34A/#ECFEFF | Electric Blue (#009CD1) + Green |
| Subscription Box Service | Vibrant & Block-based + Motion-Driven | Feature-Rich Showcase | E-commerce Analytics | #D946EF/#EA580C/#FDF4FF | Brand + Excitement colors |
| Podcast Platform | Dark Mode (OLED) + Minimalism & Swiss Style | Storytelling-Driven | Media/Entertainment Dashboard | #1E1B4B/#F97316/#0F0F23 | Dark + Audio waveform accents |
| Dating App | Vibrant & Block-based + Motion-Driven | Social Proof-Focused | User Behavior Analytics | #E11D48/#EA580C/#FFF1F2 | Warm + Romantic (Pink/Red gradients) |
| Micro-Credentials/Badges Platform | Minimalism & Swiss Style + Flat Design | Trust & Authority | Education Dashboard | #0369A1/#A16207/#F0F9FF | Trust Blue + Gold (#FFD700) |
| Knowledge Base/Documentation | Minimalism & Swiss Style + Accessible & Ethical | FAQ/Documentation | N/A - Documentation focused | #475569/#2563EB/#F8FAFC | Clean hierarchy + minimal color |
| Hyperlocal Services | Minimalism & Swiss Style + Vibrant & Block-based | Conversion-Optimized | Drill-Down Analytics + Map | #059669/#EA580C/#ECFDF5 | Location markers + Trust colors |
| Beauty/Spa/Wellness Service | Soft UI Evolution + Neumorphism | Hero-Centric Design + Social Proof | User Behavior Analytics | #EC4899/#8B5CF6/#FDF2F8 | Soft pastels (Pink #FFB6C1 Sage #90EE90) + Cr… |
| Luxury/Premium Brand | Liquid Glass + Glassmorphism | Storytelling-Driven + Feature-Rich | Sales Intelligence Dashboard | #1C1917/#A16207/#FAFAF9 | Black + Gold (#FFD700) + White + Minimal acce… |
| Restaurant/Food Service | Vibrant & Block-based + Motion-Driven | Hero-Centric Design + Conversion | N/A - Booking focused | #DC2626/#A16207/#FEF2F2 | Warm colors (Orange Red Brown) + appetizing i… |
| Fitness/Gym App | Vibrant & Block-based + Dark Mode (OLED) | Feature-Rich Showcase | User Behavior Analytics | #F97316/#22C55E/#1F2937 | Energetic (Orange #FF6B35 Electric Blue) + Da… |
| Real Estate/Property | Glassmorphism + Minimalism & Swiss Style | Hero-Centric Design + Feature-Rich | Sales Intelligence Dashboard | #0F766E/#0369A1/#F0FDFA | Trust Blue (#0077B6) + Gold accents + White |
| Travel/Tourism Agency | Aurora UI + Motion-Driven | Storytelling-Driven + Hero-Centric | Booking Analytics | #0EA5E9/#EA580C/#F0F9FF | Vibrant destination colors + Sky Blue + Warm… |
| Hotel/Hospitality | Liquid Glass + Minimalism & Swiss Style | Hero-Centric Design + Social Proof | Revenue Management Dashboard | #1E3A8A/#A16207/#F8FAFC | Warm neutrals + Gold (#D4AF37) + Brand accent |
| Wedding/Event Planning | Soft UI Evolution + Aurora UI | Storytelling-Driven + Social Proof | N/A - Planning focused | #DB2777/#A16207/#FDF2F8 | Soft Pink (#FFD6E0) + Gold + Cream + Sage |
| Legal Services | Accessible & Ethical + Minimalism & Swiss Style | Trust & Authority + Minimal | Case Management Dashboard | #1E3A8A/#B45309/#F8FAFC | Navy Blue (#1E3A5F) + Gold + White |
| Insurance Platform | Minimalism & Swiss Style + Flat Design | Conversion-Optimized + Trust | Claims Analytics Dashboard | #0369A1/#16A34A/#F0F9FF | Trust Blue (#0066CC) + Green (security) + Neu… |
| Banking/Traditional Finance | Minimalism & Swiss Style + Accessible & Ethical | Trust & Authority + Feature-Rich | Financial Dashboard | #0F172A/#A16207/#F8FAFC | Navy (#0A1628) + Trust Blue + Gold accents |
| Online Course/E-learning | Claymorphism + Vibrant & Block-based | Feature-Rich Showcase + Social Proof | Education Dashboard | #0D9488/#EA580C/#F0FDFA | Vibrant learning colors + Progress green |
| Non-profit/Charity | Accessible & Ethical + Organic Biophilic | Storytelling-Driven + Trust | Donation Analytics Dashboard | #0891B2/#EA580C/#ECFEFF | Cause-related colors + Trust + Warm |
| Music Streaming | Dark Mode (OLED) + Vibrant & Block-based | Feature-Rich Showcase | Media/Entertainment Dashboard | #1E1B4B/#22C55E/#0F0F23 | Dark (#121212) + Vibrant accents + Album art… |
| Video Streaming/OTT | Dark Mode (OLED) + Motion-Driven | Hero-Centric Design + Feature-Rich | Media/Entertainment Dashboard | #0F0F23/#E11D48/#000000 | Dark bg + Content poster colors + Brand accen… |
| Job Board/Recruitment | Flat Design + Minimalism & Swiss Style | Conversion-Optimized + Feature-Rich | HR Analytics Dashboard | #0369A1/#16A34A/#F0F9FF | Professional Blue + Success Green + Neutral |
| Marketplace (P2P) | Vibrant & Block-based + Flat Design | Feature-Rich Showcase + Social Proof | E-commerce Analytics | #7C3AED/#16A34A/#FAF5FF | Trust colors + Category colors + Success gree… |
| Logistics/Delivery | Minimalism & Swiss Style + Flat Design | Feature-Rich Showcase + Conversion | Real-Time Monitoring + Route Analytics | #2563EB/#EA580C/#EFF6FF | Blue (#2563EB) + Orange (tracking) + Green (d… |
| Agriculture/Farm Tech | Organic Biophilic + Flat Design | Feature-Rich Showcase + Trust | IoT Sensor Dashboard | #15803D/#A16207/#F0FDF4 | Earth Green (#4A7C23) + Brown + Sky Blue |
| Construction/Architecture | Minimalism & Swiss Style + 3D & Hyperrealism | Hero-Centric Design + Feature-Rich | Project Management Dashboard | #64748B/#EA580C/#F8FAFC | Grey (#4A4A4A) + Orange (safety) + Blueprint… |
| Automotive/Car Dealership | Motion-Driven + 3D & Hyperrealism | Hero-Centric Design + Feature-Rich | Sales Intelligence Dashboard | #1E293B/#DC2626/#F8FAFC | Brand colors + Metallic accents + Dark/Light |
| Photography Studio | Motion-Driven + Minimalism & Swiss Style | Storytelling-Driven + Hero-Centric | N/A - Portfolio focused | #18181B/#F8FAFC/#000000 | Black + White + Minimal accent |
| Coworking Space | Vibrant & Block-based + Glassmorphism | Hero-Centric Design + Feature-Rich | Occupancy Dashboard | #F59E0B/#2563EB/#FFFBEB | Energetic colors + Wood tones + Brand accent |
| Home Services (Plumber/Electrician) | Flat Design + Accessible & Ethical | Conversion-Optimized + Trust | Service Analytics | #1E40AF/#EA580C/#EFF6FF | Trust Blue + Safety Orange + Professional gre… |
| Childcare/Daycare | Claymorphism + Vibrant & Block-based | Social Proof-Focused + Trust | Parent Dashboard | #F472B6/#16A34A/#FDF2F8 | Playful pastels + Safe colors + Warm accents |
| Senior Care/Elderly | Accessible & Ethical + Soft UI Evolution | Trust & Authority + Social Proof | Healthcare Analytics | #0369A1/#16A34A/#F0F9FF | Calm Blue + Warm neutrals + Large text |
| Medical Clinic | Accessible & Ethical + Minimalism & Swiss Style | Trust & Authority + Conversion | Healthcare Analytics | #0891B2/#16A34A/#F0FDFA | Medical Blue (#0077B6) + Trust White + Calm G… |
| Pharmacy/Drug Store | Flat Design + Accessible & Ethical | Conversion-Optimized + Trust | Inventory Dashboard | #15803D/#0369A1/#F0FDF4 | Pharmacy Green + Trust Blue + Clean White |
| Dental Practice | Soft UI Evolution + Minimalism & Swiss Style | Social Proof-Focused + Conversion | Patient Analytics | #0EA5E9/#0EA5E9/#F0F9FF | Fresh Blue + White + Smile Yellow accent |
| Veterinary Clinic | Claymorphism + Accessible & Ethical | Social Proof-Focused + Trust | Pet Health Dashboard | #0D9488/#EA580C/#F0FDFA | Caring Blue + Pet-friendly colors + Warm acce… |
| Florist/Plant Shop | Organic Biophilic + Vibrant & Block-based | Hero-Centric Design + Conversion | E-commerce Analytics | #15803D/#EC4899/#F0FDF4 | Natural Green + Floral pinks/purples + Earth… |
| Bakery/Cafe | Vibrant & Block-based + Soft UI Evolution | Hero-Centric Design + Conversion | N/A - Order focused | #92400E/#92400E/#FEF3C7 | Warm Brown + Cream + Appetizing accents |
| Brewery/Winery | Motion-Driven + Vintage Analog / Retro Film | Storytelling-Driven + Hero-Centric | N/A - E-commerce focused | #7C2D12/#A16207/#FEF2F2 | Deep amber/burgundy + Gold + Craft aesthetic |
| Airline | Minimalism & Swiss Style + Glassmorphism | Conversion-Optimized + Feature-Rich | Operations Dashboard | #1E3A8A/#EA580C/#EFF6FF | Sky Blue + Brand colors + Trust accents |
| News/Media Platform | Minimalism & Swiss Style + Flat Design | Hero-Centric Design + Feature-Rich | Media Analytics Dashboard | #DC2626/#1E40AF/#FEF2F2 | Brand colors + High contrast + Category color… |
| Magazine/Blog | Swiss Modernism 2.0 + Motion-Driven | Storytelling-Driven + Hero-Centric | Content Analytics | #18181B/#EC4899/#FAFAFA | Editorial colors + Brand primary + Clean whit… |
| Freelancer Platform | Flat Design + Minimalism & Swiss Style | Feature-Rich Showcase + Conversion | Marketplace Analytics | #6366F1/#16A34A/#EEF2FF | Professional Blue + Success Green + Neutral |
| Marketing Agency | Brutalism + Motion-Driven | Storytelling-Driven + Feature-Rich | Campaign Analytics | #EC4899/#0891B2/#FDF2F8 | Bold brand colors + Creative freedom |
| Event Management | Vibrant & Block-based + Motion-Driven | Hero-Centric Design + Feature-Rich | Event Analytics | #7C3AED/#EA580C/#FAF5FF | Event theme colors + Excitement accents |
| Membership/Community | Vibrant & Block-based + Soft UI Evolution | Social Proof-Focused + Conversion | Community Analytics | #7C3AED/#16A34A/#FAF5FF | Community brand colors + Engagement accents |
| Newsletter Platform | Minimalism & Swiss Style + Flat Design | Minimal & Direct + Conversion | Email Analytics | #0369A1/#EA580C/#F0F9FF | Brand primary + Clean white + CTA accent |
| Digital Products/Downloads | Vibrant & Block-based + Motion-Driven | Feature-Rich Showcase + Conversion | E-commerce Analytics | #6366F1/#16A34A/#EEF2FF | Product category colors + Brand + Success gre… |
| Church/Religious Organization | Accessible & Ethical + Soft UI Evolution | Hero-Centric Design + Social Proof | N/A - Community focused | #7C3AED/#A16207/#FAF5FF | Warm Gold + Deep Purple/Blue + White |
| Sports Team/Club | Vibrant & Block-based + Motion-Driven | Hero-Centric Design + Feature-Rich | Performance Analytics | #DC2626/#DC2626/#FEF2F2 | Team colors + Energetic accents |
| Museum/Gallery | Minimalism & Swiss Style + Motion-Driven | Storytelling-Driven + Feature-Rich | Visitor Analytics | #18181B/#18181B/#FAFAFA | Art-appropriate neutrals + Exhibition accents |
| Theater/Cinema | Dark Mode (OLED) + Motion-Driven | Hero-Centric Design + Conversion | Booking Analytics | #1E1B4B/#CA8A04/#0F0F23 | Dark + Spotlight accents + Gold |
| Language Learning App | Claymorphism + Vibrant & Block-based | Feature-Rich Showcase + Social Proof | Learning Analytics | #4F46E5/#16A34A/#EEF2FF | Playful colors + Progress indicators + Countr… |
| Coding Bootcamp | Dark Mode (OLED) + Minimalism & Swiss Style | Feature-Rich Showcase + Social Proof | Student Analytics | #0F172A/#22C55E/#020617 | Code editor colors + Brand + Success green |
| Cybersecurity Platform | Cyberpunk UI + Dark Mode (OLED) | Trust & Authority + Real-Time | Real-Time Monitoring + Heat Map | #00FF41/#FF3333/#000000 | Matrix Green + Deep Black + Terminal feel |
| Developer Tool / IDE | Dark Mode (OLED) + Minimalism & Swiss Style | Minimal & Direct + Documentation | Real-Time Monitor + Terminal | #1E293B/#22C55E/#0F172A | Dark syntax theme colors + Blue focus |
| Biotech / Life Sciences | Glassmorphism + Biomimetic / Organic 2.0 | Storytelling-Driven + Research | Data-Dense + Predictive | #0EA5E9/#059669/#F0F9FF | Sterile White + DNA Blue + Life Green |
| Space Tech / Aerospace | HUD / Sci-Fi FUI + Dark Mode (OLED) | Immersive Experience + Hero | Real-Time Monitoring + 3D | #F8FAFC/#3B82F6/#0B0B10 | Deep Space Black + Star White + Metallic |
| Architecture / Interior | Exaggerated Minimalism + 3D & Hyperrealism | Portfolio Grid + Visuals | Project Management + Gallery | #171717/#A16207/#FFFFFF | Monochrome + Gold Accent + High Imagery |
| Quantum Computing Interface | HUD / Sci-Fi FUI + Dark Mode (OLED) | Immersive/Interactive Experience | 3D Spatial Data + Real-Time Monitor | #00FFFF/#FF00FF/#050510 | Quantum Blue #00FFFF + Deep Black + Interfere… |
| Biohacking / Longevity App | Biomimetic / Organic 2.0 | Data-Dense + Storytelling | Real-Time Monitor + Biological Data | #FF4D4D/#059669/#F5F5F7 | Cellular Pink/Red + DNA Blue + Clean White |
| Autonomous Drone Fleet Manager | HUD / Sci-Fi FUI | Real-Time Monitor | Geographic + Real-Time | #00FF41/#FF3333/#0D1117 | Tactical Green #00FF00 + Alert Red + Map Dark |
| Generative Art Platform | Minimalism & Swiss Style + Gen Z Chaos / Maximalism | Bento Grid Showcase | Gallery / Portfolio | #18181B/#EC4899/#FAFAFA | Neutral #F5F5F5 (Canvas) + User Content |
| Spatial Computing OS / App | Spatial UI (VisionOS) | Immersive/Interactive Experience | Spatial Dashboard | #FFFFFF/#FFFFFF/#888888 | Frosted Glass + System Colors + Depth |
| Sustainable Energy / Climate Tech | Organic Biophilic + E-Ink / Paper | Interactive Demo + Data | Energy/Utilities Dashboard | #059669/#059669/#ECFDF5 | Earth Green + Sky Blue + Solar Yellow |
| Personal Finance Tracker | Glassmorphism + Dark Mode (OLED) | Interactive Product Demo | Financial Dashboard | #1E40AF/#059669/#0F172A | Calm blue + success green + alert red + chart… |
| Chat & Messaging App | Minimalism & Swiss Style + Micro-interactions | Feature-Rich Showcase + Demo | User Behavior Analytics | #2563EB/#059669/#FFFFFF | Brand primary + bubble contrast (sender/recei… |
| Notes & Writing App | Minimalism & Swiss Style + Flat Design | Minimal & Direct | N/A - Editor focused | #78716C/#D97706/#FFFBEB | Clean white/cream + minimal accent + editor s… |
| Habit Tracker | Claymorphism + Vibrant & Block-based | Social Proof-Focused + Demo | User Behavior Analytics | #D97706/#059669/#FFFBEB | Streak warm (amber/orange) + progress green +… |
| Food Delivery / On-Demand | Vibrant & Block-based + Motion-Driven | Hero-Centric Design + Feature-Rich | Real-Time Monitoring + Map | #EA580C/#2563EB/#FFF7ED | Appetizing warm (orange/red) + trust blue + m… |
| Ride Hailing / Transportation | Minimalism & Swiss Style + Glassmorphism | Conversion-Optimized + Demo | Real-Time Monitoring + Map | #1E293B/#2563EB/#0F172A | Brand primary + map neutral + status indicato… |
| Recipe & Cooking App | Claymorphism + Vibrant & Block-based | Hero-Centric Design + Feature-Rich | N/A - Content focused | #9A3412/#059669/#FFFBEB | Warm food tones (terracotta/sage/cream) + app… |
| Meditation & Mindfulness | Neumorphism + Soft UI Evolution | Storytelling-Driven + Social Proof | User Behavior Analytics | #7C3AED/#059669/#FAF5FF | Ultra-calm pastels (lavender/sage/sky) + brea… |
| Weather App | Glassmorphism + Aurora UI | Hero-Centric Design | N/A - Utility focused | #0284C7/#F59E0B/#F0F9FF | Atmospheric gradients (sky blue → sunset → st… |
| Diary & Journal App | Soft UI Evolution + Minimalism & Swiss Style | Storytelling-Driven | N/A - Personal focused | #92400E/#6366F1/#FFFBEB | Warm paper tones (cream/linen) + muted ink +… |
| CRM & Client Management | Flat Design + Minimalism & Swiss Style | Feature-Rich Showcase + Demo | Sales Intelligence Dashboard | #2563EB/#059669/#F8FAFC | Professional blue + pipeline stage colors + c… |
| Inventory & Stock Management | Flat Design + Minimalism & Swiss Style | Feature-Rich Showcase | Real-Time Monitoring + Data-Dense | #334155/#059669/#F8FAFC | Functional neutral + status traffic-light (gr… |
| Flashcard & Study Tool | Claymorphism + Micro-interactions | Feature-Rich Showcase + Demo | Learning Analytics | #7C3AED/#059669/#FAF5FF | Playful primary + correct green + incorrect r… |
| Booking & Appointment App | Soft UI Evolution + Flat Design | Conversion-Optimized | Drill-Down Analytics | #0284C7/#059669/#F0F9FF | Trust blue + available green + booked grey +… |
| Invoice & Billing Tool | Minimalism & Swiss Style + Flat Design | Conversion-Optimized + Trust | Financial Dashboard | #1E3A5F/#059669/#F8FAFC | Professional navy + paid green + overdue red… |
| Grocery & Shopping List | Flat Design + Vibrant & Block-based | Minimal & Direct + Demo | N/A - List focused | #059669/#D97706/#ECFDF5 | Fresh green + food-category colors + checkmar… |
| Timer & Pomodoro | Minimalism & Swiss Style + Neumorphism | Minimal & Direct | N/A - Utility focused | #DC2626/#059669/#0F172A | High-contrast on dark + focus red/amber + bre… |
| Parenting & Baby Tracker | Claymorphism + Soft UI Evolution | Social Proof-Focused + Trust | User Behavior Analytics | #EC4899/#0284C7/#FDF2F8 | Soft pastels (baby pink/sky blue/mint/peach)… |
| Scanner & Document Manager | Minimalism & Swiss Style + Flat Design | Feature-Rich Showcase + Demo | N/A - Tool focused | #1E293B/#2563EB/#F8FAFC | Clean white + camera viewfinder accent + file… |
| Calendar & Scheduling App | Flat Design + Micro-interactions | Feature-Rich Showcase + Demo | N/A - Calendar focused | #2563EB/#059669/#F8FAFC | Clean blue + event category accent colors + s… |
| Password Manager | Minimalism & Swiss Style + Accessible & Ethical | Trust & Authority + Feature-Rich | N/A - Vault focused | #1E3A5F/#059669/#0F172A | Trust blue + security green + dark neutral |
| Expense Splitter / Bill Split | Flat Design + Vibrant & Block-based | Minimal & Direct + Demo | N/A - Balance focused | #059669/#DC2626/#F8FAFC | Success green + alert red + neutral grey + av… |
| Voice Recorder & Memo | Minimalism & Swiss Style + AI-Native UI | Interactive Product Demo + Minimal | N/A - Recording focused | #DC2626/#2563EB/#FFFFFF | Clean white + recording red + waveform accent |
| Bookmark & Read-Later | Minimalism & Swiss Style + Flat Design | Minimal & Direct + Demo | N/A - List focused | #D97706/#2563EB/#FFFBEB | Paper warm white + ink neutral + minimal acce… |
| Translator App | Flat Design + AI-Native UI | Feature-Rich Showcase + Interactive Demo | N/A - Utility focused | #2563EB/#EA580C/#F8FAFC | Global blue + neutral grey + language flag ac… |
| Calculator & Unit Converter | Neumorphism + Minimalism & Swiss Style | Minimal & Direct | N/A - Utility focused | #EA580C/#2563EB/#1C1917 | Dark functional + orange operation keys + cle… |
| Alarm & World Clock | Dark Mode (OLED) + Minimalism & Swiss Style | Minimal & Direct | N/A - Utility focused | #D97706/#6366F1/#0F172A | Deep dark + ambient glow accent + timezone gr… |
| File Manager & Transfer | Flat Design + Minimalism & Swiss Style | Feature-Rich Showcase + Demo | N/A - File tree focused | #2563EB/#D97706/#F8FAFC | Functional neutral + file type color coding (… |
| Email Client | Flat Design + Minimalism & Swiss Style | Feature-Rich Showcase + Demo | N/A - Inbox focused | #2563EB/#DC2626/#FFFFFF | Clean white + brand primary + priority red +… |
| Casual Puzzle Game | Claymorphism + Vibrant & Block-based | Feature-Rich Showcase + Social Proof | N/A - Game focused | #EC4899/#F59E0B/#FDF2F8 | Cheerful pastels + progression gradient + rew… |
| Trivia & Quiz Game | Vibrant & Block-based + Micro-interactions | Feature-Rich Showcase + Social Proof | Leaderboard Analytics | #2563EB/#F59E0B/#EFF6FF | Energetic blue + correct green + incorrect re… |
| Card & Board Game | 3D & Hyperrealism + Flat Design | Feature-Rich Showcase | N/A - Game focused | #15803D/#D97706/#0F172A | Game-theme felt green + dark wood + card back… |
| Idle & Clicker Game | Vibrant & Block-based + Motion-Driven | Feature-Rich Showcase | N/A - Progress focused | #D97706/#7C3AED/#FFFBEB | Coin gold + upgrade blue + prestige purple +… |
| Word & Crossword Game | Minimalism & Swiss Style + Flat Design | Minimal & Direct + Demo | N/A - Game focused | #15803D/#D97706/#FFFFFF | Clean white + warm letter tiles + success gre… |
| Arcade & Retro Game | Pixel Art + Retro-Futurism | Feature-Rich Showcase + Hero-Centric | N/A - Score focused | #DC2626/#22C55E/#0F172A | Neon on black + pixel palette + score gold +… |
| Photo Editor & Filters | Minimalism & Swiss Style + Dark Mode (OLED) | Feature-Rich Showcase + Interactive Demo | N/A - Editor focused | #7C3AED/#0891B2/#0F172A | Dark editor background + vibrant filter previ… |
| Short Video Editor | Dark Mode (OLED) + Motion-Driven | Feature-Rich Showcase + Hero-Centric | N/A - Timeline editor focused | #EC4899/#2563EB/#0F172A | Dark background + timeline track accent color… |
| Drawing & Sketching Canvas | Minimalism & Swiss Style + Dark Mode (OLED) | Interactive Product Demo + Storytelling | N/A - Canvas focused | #7C3AED/#0891B2/#1C1917 | Neutral canvas + full-spectrum color picker +… |
| Music Creation & Beat Maker | Dark Mode (OLED) + Motion-Driven | Interactive Product Demo + Storytelling | N/A - DAW focused | #7C3AED/#22C55E/#0F172A | Dark studio background + track colors rainbow… |
| Meme & Sticker Maker | Vibrant & Block-based + Flat Design | Feature-Rich Showcase + Social Proof | N/A - Creator focused | #EC4899/#2563EB/#FFFFFF | Bold primary + comedic yellow + viral red + h… |
| AI Photo & Avatar Generator | AI-Native UI + Aurora UI | Feature-Rich Showcase + Social Proof | N/A - Generation focused | #7C3AED/#EC4899/#FAF5FF | AI purple + aurora gradients + before/after n… |
| Link-in-Bio Page Builder | Vibrant & Block-based + Bento Box Grid | Conversion-Optimized + Social Proof | Analytics (click tracking) | #2563EB/#EC4899/#FFFFFF | Brand-customizable + accent link color + clea… |
| Wardrobe & Outfit Planner | Minimalism & Swiss Style + Motion-Driven | Storytelling-Driven + Feature-Rich | N/A - Wardrobe focused | #BE185D/#D97706/#FDF2F8 | Clean fashion neutral + full clothes color pa… |
| Plant Care Tracker | Organic Biophilic + Soft UI Evolution | Storytelling-Driven + Social Proof | N/A - Plant collection focused | #15803D/#D97706/#F0FDF4 | Nature greens + earth brown + sunny yellow re… |
| Book & Reading Tracker | Swiss Modernism 2.0 + Minimalism & Swiss Style | Social Proof-Focused + Feature-Rich | N/A - Library focused | #78716C/#D97706/#FFFBEB | Warm paper white + ink brown + reading progre… |
| Couple & Relationship App | Aurora UI + Soft UI Evolution | Storytelling-Driven + Social Proof | N/A - Couple focused | #BE185D/#DC2626/#FDF2F8 | Warm romantic pink/rose + soft gradient + mem… |
| Family Calendar & Chores | Flat Design + Claymorphism | Feature-Rich Showcase + Social Proof | N/A - Family hub focused | #2563EB/#D97706/#F8FAFC | Warm playful + member color coding + chore co… |
| Mood Tracker | Soft UI Evolution + Minimalism & Swiss Style | Storytelling-Driven + Social Proof | N/A - Mood chart focused | #7C3AED/#D97706/#FAF5FF | Emotion gradient (blue sad to yellow happy) +… |
| Gift & Wishlist | Vibrant & Block-based + Soft UI Evolution | Minimal & Direct + Conversion | N/A - List focused | #DC2626/#EC4899/#FFF1F2 | Celebration warm pink/gold/red + category col… |
| Running & Cycling GPS | Dark Mode (OLED) + Vibrant & Block-based | Feature-Rich Showcase + Social Proof | Performance Analytics | #EA580C/#059669/#0F172A | Energetic orange + map accent + pace zones (g… |
| Yoga & Stretching Guide | Organic Biophilic + Soft UI Evolution | Storytelling-Driven + Social Proof | N/A - Session focused | #6B7280/#0891B2/#F5F5F0 | Earth calming sage/terracotta/cream + breathi… |
| Sleep Tracker | Dark Mode (OLED) + Neumorphism | Feature-Rich Showcase + Social Proof | Healthcare Analytics | #4338CA/#7C3AED/#0F172A | Deep midnight blue + stars/moon accent + slee… |
| Calorie & Nutrition Counter | Flat Design + Vibrant & Block-based | Feature-Rich Showcase + Social Proof | Healthcare Analytics | #059669/#EA580C/#ECFDF5 | Healthy green + macro colors (protein blue, c… |
| Period & Cycle Tracker | Soft UI Evolution + Aurora UI | Social Proof-Focused + Trust | Healthcare Analytics | #BE185D/#7C3AED/#FDF2F8 | Rose/blush + lavender + fertility green + sof… |
| Medication & Pill Reminder | Accessible & Ethical + Flat Design | Trust & Authority + Feature-Rich | N/A - Schedule focused | #0284C7/#DC2626/#F0F9FF | Medical trust blue + missed alert red + taken… |
| Water & Hydration Reminder | Claymorphism + Vibrant & Block-based | Minimal & Direct + Demo | N/A - Daily goal focused | #0284C7/#0891B2/#F0F9FF | Refreshing blue + water wave animation + goal… |
| Fasting & Intermittent Timer | Minimalism & Swiss Style + Dark Mode (OLED) | Feature-Rich Showcase + Social Proof | N/A - Timer focused | #6366F1/#059669/#0F172A | Fasting deep blue/purple + eating window gree… |
| Anonymous Community / Confession | Dark Mode (OLED) + Minimalism & Swiss Style | Social Proof-Focused + Feature-Rich | User Behavior Analytics | #475569/#0891B2/#0F172A | Dark protective + subtle gradient + upvote gr… |
| Local Events & Discovery | Vibrant & Block-based + Motion-Driven | Hero-Centric Design + Feature-Rich | Event Analytics | #EA580C/#2563EB/#FFF7ED | City vibrant + event category colors + map ac… |
| Study Together / Virtual Coworking | Minimalism & Swiss Style + Soft UI Evolution | Social Proof-Focused + Feature-Rich | User Behavior Analytics | #2563EB/#059669/#F8FAFC | Calm focus blue + session progress indicator… |
| Coding Challenge & Practice | Dark Mode (OLED) + Cyberpunk UI | Feature-Rich Showcase + Social Proof | Student Analytics | #22C55E/#D97706/#0F172A | Code editor dark + success green + difficulty… |
| Kids Learning (ABC & Math) | Claymorphism + Vibrant & Block-based | Social Proof-Focused + Trust | Parent Dashboard | #2563EB/#EC4899/#EFF6FF | Bright primary + child-safe pastels + reward… |
| Music Instrument Learning | Vibrant & Block-based + Motion-Driven | Interactive Product Demo + Social Proof | Learning Analytics | #DC2626/#D97706/#FFFBEB | Musical warm deep red/brown + note color syst… |
| Parking Finder | Minimalism & Swiss Style + Glassmorphism | Conversion-Optimized + Feature-Rich | Real-Time Monitoring + Map | #2563EB/#DC2626/#F0F9FF | Trust blue + available green + occupied red +… |
| Public Transit Guide | Flat Design + Accessible & Ethical | Feature-Rich Showcase + Interactive Demo | Real-Time Monitoring + Map | #2563EB/#EA580C/#F8FAFC | Transit brand line colors + real-time indicat… |
| Road Trip Planner | Aurora UI + Organic Biophilic | Storytelling-Driven + Hero-Centric | N/A - Trip focused | #EA580C/#D97706/#FFF7ED | Adventure warm sunset orange + map teal + sto… |
| VPN & Privacy Tool | Minimalism & Swiss Style + Dark Mode (OLED) | Trust & Authority + Conversion-Optimized | N/A - Connection focused | #1E3A5F/#22C55E/#0F172A | Dark shield blue + connected green + disconne… |
| Emergency SOS & Safety | Accessible & Ethical + Flat Design | Trust & Authority + Social Proof | N/A - Safety focused | #DC2626/#2563EB/#FFF1F2 | Alert red + safety blue + location green + hi… |
| Wallpaper & Theme App | Vibrant & Block-based + Aurora UI | Feature-Rich Showcase + Social Proof | N/A - Gallery focused | #7C3AED/#2563EB/#FAF5FF | Content-driven + trending aesthetic palettes… |
| White Noise & Ambient Sound | Minimalism & Swiss Style + Dark Mode (OLED) | Minimal & Direct + Social Proof | N/A - Player focused | #475569/#4338CA/#0F172A | Calming dark + ambient texture visual + subtl… |
| Home Decoration & Interior Design | Minimalism & Swiss Style + 3D Product Preview | Storytelling-Driven + Feature-Rich | N/A - Project focused | #78716C/#D97706/#FAF5F2 | Neutral interior palette + material texture a… |
| Academic Journal / Scholarly Publishing | Swiss Modernism 2.0 + Minimalism & Swiss Style | Content-Index + Search | N/A - Publication focused | #1E3A5F/#B45309/#F8FAFC | Trust navy + White + Citation blue + Serif ac… |
| API Developer Portal | Accessible & Ethical + Minimalism & Swiss Style | Quick Start + Interactive Docs | N/A - Documentation focused | #0F172A/#22C55E/#020617 | Dark code theme + Brand accent + Syntax color… |
| Forum / Discussion Board | Dark Mode (OLED) + Minimalism & Swiss Style | Feed + Thread View | N/A - Discussion focused | #475569/#2563EB/#F8FAFC | Dark neutral + topic accent colors + unread i… |
| Directory / Listing Site | Flat Design + Vibrant & Block-based | Filter-Heavy Grid + Map | N/A - Listing focused | #059669/#D97706/#ECFDF5 | Neutral bg + category color chips + map accen… |
| Status Page / Incident Management | Data-Dense Dashboard + Real-Time Monitoring | Timeline + Severity Indicators | Real-Time Monitoring + Timeline | #16A34A/#DC2626/#F0FDF4 | Status green + incident red + maintenance amb… |
| Wiki / Encyclopedia | Minimalism & Swiss Style + Flat Design | Search-First + Hierarchical Navigation | N/A - Reference focused | #1E3A8A/#7C3AED/#F8FAFC | Clean white + link blue + heading hierarchy +… |
| Auction Platform | Dark Mode (OLED) + Motion-Driven | Live Auction Feed + Countdown | N/A - Auction focused | #0F172A/#16A34A/#020617 | Dark bg + bid green + outbid red + countdown… |
| Changelog / Release Notes | Minimalism & Swiss Style + Flat Design | Timeline + Version List | N/A - Documentation focused | #475569/#059669/#F8FAFC | Neutral bg + version badge colors (feat=green… |
| Citizen Science Platform | Organic Biophilic + Vibrant & Block-based | Storytelling-Driven + Social Proof | Project Participation Dashboard | #15803D/#D97706/#F0FDF4 | Earth green + discovery orange + volunteer ba… |
| Classifieds / Buy-Sell | Flat Design + Vibrant & Block-based | Filter-Heavy Grid + Map | N/A - Listing focused | #2563EB/#16A34A/#EFF6FF | Neutral bg + price green + category chips + v… |
| Conference / Symposium Landing Page | Swiss Modernism 2.0 + Minimalism & Swiss Style | Hero + Agenda + CFP | N/A - Event focused | #1E3A5F/#A16207/#F8FAFC | Academic navy + track color chips + gold keyn… |
| Crowdfunding Platform | Vibrant & Block-based + Motion-Driven | Storytelling-Driven + Social Proof | Campaign Analytics Dashboard | #D97706/#16A34A/#FFFBEB | Brand primary + funding progress green + urge… |
| Digital Signage / Kiosk | Minimalism & Swiss Style + Dark Mode (OLED) | Full-Screen Immersive | N/A - Display focused | #0F172A/#EF4444/#020617 | High contrast + brand accent + touch target e… |
| E-signature / Document Workflow | Accessible & Ethical + Minimalism & Swiss Style | Feature-Rich Showcase + Conversion | Document Pipeline Dashboard | #1E3A5F/#16A34A/#F8FAFC | Trust navy + signature green + pending amber… |
| Feature Flag / Config Management | Dark Mode (OLED) + Data-Dense Dashboard | Feature List + Toggle Panel | N/A - Config focused | #0F172A/#16A34A/#020617 | Dark bg + enabled green + disabled grey + exp… |
| Government Portal / Civic Services | Accessible & Ethical + Inclusive Design | Service Directory + Search | N/A - Service focused | #1E40AF/#16A34A/#EFF6FF | Professional blue + accessibility high contra… |
| Grant / Funding Portal | Accessible & Ethical + Minimalism & Swiss Style | Opportunity Grid + Search | Application Tracking Dashboard | #1E3A5F/#16A34A/#F8FAFC | Institution navy + funding green + deadline r… |
| LMS (Learning Management System) | Flat Design + Accessible & Ethical | Dashboard + Course Grid | Education Analytics Dashboard | #0D9488/#D97706/#F0FDFA | Calm blue + course category colors + grade gr… |
| No-code / Low-code Builder | Vibrant & Block-based + Bento Box Grid | Interactive Product Demo | App Builder Workspace | #7C3AED/#EC4899/#FAF5FF | Brand primary + component palette colors + ca… |
| Open Source Project Landing | Dark Mode (OLED) + Minimalism & Swiss Style | Hero + Install + Contribute | Contributor Analytics Dashboard | #0F172A/#A16207/#020617 | Dark bg + language color bar + star gold + fo… |
| Patient Portal / Health Records | Minimalism & Swiss Style + Accessible & Ethical | Health Summary Dashboard | Healthcare Analytics | #0284C7/#16A34A/#F0F9FF | Clinical blue + health green + alert red + ca… |
| Patent / IP Database | Swiss Modernism 2.0 + Minimalism & Swiss Style | Search-First + Results Grid | N/A - Search focused | #475569/#A16207/#F8FAFC | Formal neutral + patent type chips + status b… |
| Q&A Community Platform | Minimalism & Swiss Style + Flat Design | Feed + Thread View | Community Analytics Dashboard | #2563EB/#D97706/#F8FAFC | Clean white + upvote orange + accepted green… |
| Research Lab / University Department | Swiss Modernism 2.0 + Minimalism & Swiss Style | Overview + People + Publications | N/A - Academic focused | #1E3A5F/#A16207/#F8FAFC | Institutional navy + white + research area ac… |
| Resume / CV Builder | Minimalism & Swiss Style + Flat Design | Interactive Product Demo + CTA | Template Selection Gallery | #1E3A5F/#16A34A/#F8FAFC | Professional navy + section accent + success… |
| Review Platform | Flat Design + Vibrant & Block-based | Hero + Rating Summary + Review Feed | Review Analytics Dashboard | #F59E0B/#16A34A/#FFFBEB | Brand primary + star gold + positive green +… |
| RPA / Automation Dashboard | Dark Mode (OLED) + Data-Dense Dashboard | Bot Fleet Dashboard | Real-Time Monitoring + Process Analytics | #0F172A/#16A34A/#020617 | Dark bg + running green + failed red + queued… |
| Survey / Form Builder | Minimalism & Swiss Style + Micro-interactions | Interactive Product Demo | Response Analytics Dashboard | #0D9488/#D97706/#F0FDFA | Clean white + question accent + progress gree… |
| Telemedicine Platform | Neumorphism + Accessible & Ethical | Trust & Authority + Conversion | Healthcare Analytics | #0891B2/#16A34A/#F0FDFA | Calm medical blue + video green + waiting amb… |
| Testimonial & Social Proof Widget | Vibrant & Block-based + Flat Design | Wall-of-Love Grid | Engagement Analytics Dashboard | #7C3AED/#F59E0B/#FAF5FF | Brand primary + quote accent + star gold + ve… |
| Ticketing / Box Office | Vibrant & Block-based + Motion-Driven | Event Grid + Seat Map | Sales Analytics Dashboard | #0F172A/#16A34A/#020617 | Event theme colors + available green + sold-o… |

## 2B. UI Styles (50 active styles)
Columns: style · keywords · effects & animation · best for · do not use for.

| Style | Keywords | Effects & animation | Best for | Do not use for |
|---|---|---|---|---|
| Minimalism & Swiss Style | Clean, simple, spacious, functional, white space, high contrast, geometric, sans-serif, grid-based, essential | Subtle hover (200-250ms), smooth transitions, sharp shadows if any, clear type hierarchy, fast loading | Enterprise apps, dashboards, documentation sites, SaaS platforms, professional t… | Creative portfolios, entertainment, playful brands, artistic experimen… |
| Neumorphism | Soft UI, embossed, debossed, convex, concave, light source, subtle depth, rounded (12-16px), monochromatic | Soft box-shadow (multiple: -5px -5px 15px, 5px 5px 15px), smooth press (150ms), inner subtle shadow | Health/wellness apps, meditation platforms, fitness trackers, minimal interactio… | Complex apps, critical accessibility, data-heavy dashboards, high-cont… |
| Glassmorphism | Frosted glass, transparent, blurred background, layered, vibrant background, light source, depth, multi-layer | Backdrop blur (10-20px), subtle border (1px solid rgba white 0.2), light reflection, Z-depth | Modern SaaS, financial dashboards, high-end corporate, lifestyle apps, modal ove… | Low-contrast backgrounds, critical accessibility, performance-limited… |
| Brutalism | Raw, unpolished, stark, high contrast, plain text, default fonts, visible borders, asymmetric, anti-design | No smooth transitions (instant), sharp corners (0px), bold typography (700+), visible grid, large blocks | Design portfolios, artistic projects, counter-culture brands, editorial/media si… | Corporate environments, conservative industries, critical accessibilit… |
| 3D & Hyperrealism | Depth, realistic textures, 3D models, spatial navigation, tactile, skeuomorphic elements, rich detail, immersi… | WebGL/Three.js 3D, realistic shadows (layers), physics lighting, parallax (3-5 layers), smooth 3D (300-400ms) | Gaming, product showcase, immersive experiences, high-end e-commerce, architectu… | Low-end mobile, performance-limited, critical accessibility, data tabl… |
| Vibrant & Block-based | Bold, energetic, playful, block layout, geometric shapes, high color contrast, duotone, modern, energetic | Large sections (48px+ gaps), animated patterns, bold hover (color shift), scroll-snap, large type (32px+), 200… | Startups, creative agencies, gaming, social media, youth-focused, entertainment… | Financial institutions, healthcare, formal business, government, conse… |
| Dark Mode (OLED) | Dark theme, low light, high contrast, deep black, midnight blue, eye-friendly, OLED, night mode, power efficie… | Minimal glow (text-shadow: 0 0 10px), dark-to-light transitions, low white emission, high readability, visible… | Night-mode apps, coding platforms, entertainment, eye-strain prevention, OLED de… | Print-first content, high-brightness outdoor, color-accuracy-critical |
| Accessible & Ethical | Accessible, inclusive interface, high contrast, large text (16px+), keyboard navigation, screen reader friendl… | Clear focus rings (3-4px), ARIA labels, skip links, responsive design, reduced motion, 44x44px touch targets | Government, healthcare, education, inclusive products, large audience, legal com… | None - accessibility universal |
| Claymorphism | Soft 3D, chunky, playful, toy-like, bubbly, thick borders (3-4px), double shadows, rounded (16-24px) | Inner+outer shadows (subtle, no hard lines), soft press (200ms ease-out), fluffy elements, smooth transitions | Educational apps, children's apps, SaaS platforms, creative tools, fun-focused… | Formal corporate, professional services, data-critical, serious/medica… |
| Aurora UI | Vibrant gradients, smooth blend, Northern Lights effect, mesh gradient, luminous, atmospheric, abstract | Large flowing CSS/SVG gradients, subtle 8-12s animations, depth via color layering, smooth morph | Modern SaaS, creative agencies, branding, music platforms, lifestyle, premium pr… | Data-heavy dashboards, critical accessibility, content-heavy where dis… |
| Retro-Futurism | Vintage sci-fi, 80s aesthetic, neon glow, geometric patterns, CRT scanlines, pixel art, cyberpunk, synthwave | CRT scanlines (::before overlay), neon glow (text-shadow+box-shadow), glitch effects (skew/offset keyframes) | Gaming, entertainment, music platforms, tech brands, artistic projects, nostalgi… | Conservative industries, critical accessibility, professional/corporat… |
| Flat Design | 2D, minimalist, bold colors, no shadows, clean lines, simple shapes, typography-focused, modern, icon-heavy | No gradients/shadows, simple hover (color/opacity shift), fast loading, clean transitions (150-200ms ease), mi… | Web apps, mobile apps, cross-platform, startup MVPs, user-friendly, SaaS, dashbo… | Complex 3D, premium/luxury, artistic portfolios, immersive experiences… |
| Skeuomorphism | Realistic, texture, depth, 3D appearance, real-world metaphors, shadows, gradients, tactile, detailed, materia… | Realistic shadows (layers), depth (perspective), texture details (noise, grain), realistic animations (300-500… | Legacy apps, gaming, immersive storytelling, premium products, luxury, realistic… | Modern enterprise, critical accessibility, low-performance, web (use F… |
| Liquid Glass | dynamic material, optical glass, translucency, lensing, refraction, fluid morphing, system navigation | Lensing and refraction, adaptive translucency, and fluid morph transitions aligned to Apple platform behavior | Apple-platform navigation, controls, and system-aligned app chrome | content layers, dense reading surfaces, or custom effects without acce… |
| Motion-Driven | Animation-heavy, microinteractions, smooth transitions, scroll effects, parallax, entrance anim, page transiti… | Scroll anim (Intersection Observer), hover (300-400ms), entrance, parallax (3-5 layers), page transitions | Portfolio sites, storytelling platforms, interactive experiences, entertainment… | Data dashboards, critical accessibility, low-power devices, content-he… |
| Micro-interactions | Small animations, gesture-based, tactile feedback, subtle animations, contextual interactions, responsive | Small hover (50-100ms), loading spinners, success/error state anim, gesture-triggered (swipe/pinch), haptic | Mobile apps, touchscreen UIs, productivity tools, user-friendly, consumer apps… | Desktop-only, critical performance, accessibility-first (alternatives… |
| Inclusive Design | Accessible, color-blind friendly, high contrast, haptic feedback, voice interaction, screen reader, enhanced c… | Haptic feedback (vibration), voice guidance, focus indicators (4px+ ring), motion options, alt content, semant… | Public services, education, healthcare, finance, government, accessible consumer… | None - accessibility universal |
| Zero Interface | Minimal visible UI, voice-first, gesture-based, AI-driven, invisible controls, predictive, context-aware, ambi… | Voice recognition UI, gesture detection, AI predictions (smooth reveal), progressive disclosure, smart suggest… | Voice assistants, AI platforms, future-forward UX, smart home, contextual comput… | Complex workflows, data-entry heavy, traditional systems, legacy suppo… |
| Soft UI Evolution | Evolved soft UI, better contrast, modern aesthetics, subtle depth, accessibility-focused, improved shadows, hy… | Improved shadows (softer than flat, clearer than neumorphism), modern (200-300ms), focus visible, measured con… | Modern enterprise apps, SaaS platforms, health/wellness, modern business tools… | Extreme minimalism, critical performance, systems without modern OS |
| Data-Dense Dashboard | Multiple charts/widgets, data tables, KPI cards, minimal padding, grid layout, space-efficient, maximum data v… | Hover tooltips, chart zoom on click, row highlighting on hover, smooth filter animations, data loading spinner… | Business intelligence dashboards, financial analytics, enterprise reporting, ope… | Marketing dashboards, consumer-facing analytics, simple reporting |
| Neubrutalism | Bold borders, black outlines, primary colors, thick shadows, no gradients, flat colors, 45° shadows, playful… | box-shadow: 4px 4px 0 #000, border: 3px solid #000, no gradients, sharp corners (0px), bold typography | Gen Z brands, startups, creative agencies, Figma-style apps, Notion-style interf… | Luxury brands, finance, healthcare, conservative industries (too playf… |
| Bento Box Grid | Modular cards, asymmetric grid, varied sizes, Apple-style, dashboard tiles, negative space, clean hierarchy, c… | grid-template with varied spans, rounded-xl (16px), subtle shadows, hover scale (1.02), smooth transitions | Dashboards, product pages, portfolios, Apple-style marketing, feature showcases… | Dense data tables, text-heavy content, real-time monitoring |
| Y2K Aesthetic | Neon pink, chrome, metallic, bubblegum, iridescent, glossy, retro-futurism, 2000s, futuristic nostalgia | linear-gradient metallic, glossy buttons, 3D chrome effects, glow animations, bubble shapes | Fashion brands, music platforms, Gen Z brands, nostalgia marketing, entertainmen… | B2B enterprise, healthcare, finance, conservative industries, elderly… |
| Cyberpunk UI | Neon, dark mode, terminal, HUD, sci-fi, glitch, dystopian, futuristic, matrix, tech noir | Neon glow (text-shadow), glitch animations (skew/offset), scanlines (::before overlay), terminal fonts | Gaming platforms, tech products, crypto apps, sci-fi applications, developer too… | Corporate enterprise, healthcare, family apps, conservative brands, el… |
| Organic Biophilic | Nature, organic shapes, green, sustainable, rounded, flowing, wellness, earthy, natural textures | Rounded corners (16-24px), organic curves (border-radius variations), natural shadows, flowing SVG shapes | Wellness apps, sustainability brands, eco products, health apps, meditation, org… | Tech-focused products, gaming, industrial, urban brands |
| AI-Native UI | Chatbot, conversational, voice, assistant, agentic, ambient, minimal chrome, streaming text, AI interactions | Typing indicators (3-dot pulse), streaming text animations, pulse animations, context cards, smooth reveals | AI products, chatbots, voice assistants, copilots, AI-powered tools, conversatio… | Traditional forms, data-heavy dashboards, print-first content |
| Memphis Design | 80s, geometric, playful, postmodern, shapes, patterns, squiggles, triangles, neon, abstract, bold | transform: rotate(), clip-path: polygon(), mix-blend-mode, repeating patterns, bold shapes | Creative agencies, music sites, youth brands, event promotion, artistic portfoli… | Corporate finance, healthcare, legal, elderly users, conservative bran… |
| Dimensional Layering | Depth, overlapping, z-index, layers, 3D, shadows, elevation, floating, cards, spatial hierarchy | z-index stacking, box-shadow elevation (4 levels), transform: translateZ(), backdrop-filter, parallax | Dashboards, card layouts, modals, navigation, product showcases, SaaS interfaces | Print-style layouts, simple blogs, low-end devices, flat design requir… |
| Exaggerated Minimalism | Bold minimalism, oversized typography, high contrast, negative space, loud minimal, statement design | font-size: clamp(3rem 10vw 12rem), font-weight: 900, letter-spacing: -0.05em, massive whitespace | Fashion, architecture, portfolios, agency landing pages, luxury brands, editoria… | E-commerce catalogs, dashboards, forms, data-heavy, elderly users, com… |
| Kinetic Typography | Motion text, animated type, moving letters, dynamic, typing effect, morphing, scroll-triggered text | @keyframes text animation, typing effect, background-clip: text, GSAP ScrollTrigger, split text | Hero sections, marketing sites, video platforms, storytelling, creative portfoli… | Long-form content, accessibility-critical, data interfaces, forms, eld… |
| Parallax Storytelling | Scroll-driven, narrative, layered scrolling, immersive, progressive disclosure, cinematic, scroll-triggered | transform: translateY(scroll), position: fixed/sticky, perspective: 1px, scroll-triggered animations | Brand storytelling, product launches, case studies, portfolios, annual reports… | E-commerce, dashboards, mobile-first, SEO-critical, accessibility-requ… |
| HUD / Sci-Fi FUI | Futuristic, technical, wireframe, neon, data, transparency, iron man, sci-fi, interface | Glow effects, scanning animations, ticker text, blinking markers, fine line drawing | Sci-fi games, space tech, cybersecurity, movie props, immersive dashboards | Standard corporate, reading heavy content, accessible public services |
| Pixel Art | Retro, 8-bit, 16-bit, gaming, blocky, nostalgic, pixelated, arcade | Frame-by-frame sprite animation, blinking cursor, instant transitions, marquee text | Indie games, retro tools, creative portfolios, nostalgia marketing, Web3/NFT | Professional corporate, modern SaaS, high-res photography sites |
| Spatial UI (VisionOS) | Glass, depth, immersion, spatial, translucent, gaze, gesture, apple, vision-pro | Parallax depth, dynamic lighting response, gaze-hover effects, smooth scale on focus | Spatial computing apps, VR/AR interfaces, immersive media, futuristic dashboards | Text-heavy documents, high-contrast requirements, non-3D capable devic… |
| E-Ink / Paper | Paper-like, matte, high contrast, texture, reading, calm, slow tech, monochrome | No motion blur, distinct page turns, grain/noise texture, sharp transitions (no fade) | Reading apps, digital newspapers, minimal journals, distraction-free writing, sl… | Gaming, video platforms, high-energy marketing, dark mode dependent ap… |
| Gen Z Chaos / Maximalism | Chaos, clutter, stickers, raw, collage, mixed media, loud, internet culture, ironic | Marquee scrolls, jitter, sticker layering, GIF overload, random placement, drag-and-drop | Gen Z lifestyle brands, music artists, creative portfolios, viral marketing, fas… | Corporate, government, healthcare, banking, serious tools |
| Biomimetic / Organic 2.0 | Nature-inspired, cellular, fluid, breathing, generative, algorithms, life-like | Breathing animations, fluid morphing, generative growth, physics-based movement | Sustainability tech, biotech, advanced health, meditation, generative art platfo… | Standard SaaS, data grids, strict corporate, accounting |
| Anti-Polish / Raw Aesthetic | Hand-drawn, collage, scanned textures, unfinished, imperfect, authentic, human, sketch, raw marks, creative pr… | No smooth transitions, hand-drawn animations, paper texture overlays, jitter effects, sketch reveal | Creative portfolios, artist sites, indie brands, handmade products, authentic st… | Corporate enterprise, fintech, healthcare, government, polished SaaS |
| Tactile Digital / Deformable UI | Jelly buttons, chrome, clay, squishy, deformable, bouncy, physical, tactile feedback, press response | Press deformation (scale + squish), bounce-back (cubic-bezier), material response, haptic-like feedback, sprin… | Modern mobile apps, playful brands, entertainment, gaming UI, consumer products… | Enterprise software, data dashboards, accessibility-critical, professi… |
| Nature Distilled | Muted earthy, skin tones, wood, soil, sand, terracotta, warmth, organic materials, handmade warmth | Subtle parallax, natural easing (ease-out), texture overlays, grain effects, soft shadows | Wellness brands, sustainable products, artisan goods, organic food, spa/beauty… | Tech startups, gaming, nightlife, corporate finance, high-energy brand… |
| Interactive Cursor Design | Custom cursor, cursor as tool, hover effects, cursor feedback, pointer transformation, cursor trail, magnetic… | Cursor scale on hover, magnetic pull to elements, cursor morphing, trail effects, blend mode cursors, click fe… | Creative portfolios, interactive experiences, agency sites, product showcases, g… | Mobile-first (no cursor), accessibility-critical, data-heavy dashboard… |
| Voice-First Multimodal | Voice UI, multimodal, audio feedback, conversational, hands-free, ambient, contextual, speech recognition | Voice waveform visualization, listening pulse, processing spinner, speak animation, smooth transitions | Voice assistants, accessibility apps, hands-free tools, smart home, automotive U… | Visual-heavy content, data entry, complex forms, noisy environments |
| 3D Product Preview | 360 product view, rotatable, zoomable, touch-to-spin, AR preview, product configurator, interactive 3D model | Drag-to-rotate, pinch-to-zoom, spin animation, AR placement, material switching, smooth orbit controls | E-commerce, furniture, fashion, automotive, electronics, jewelry, product config… | Content-heavy sites, blogs, dashboards, low-bandwidth, accessibility-c… |
| Editorial Grid / Magazine | Magazine layout, asymmetric grid, editorial typography, pull quotes, drop caps, column layout, print-inspired | Smooth scroll, reveal on scroll, parallax images, text animations, page-flip transitions | News sites, blogs, magazines, editorial content, long-form articles, journalism… | Dashboards, apps, e-commerce catalogs, real-time data, short-form cont… |
| Vintage Analog / Retro Film | Film grain, VHS, cassette tape, polaroid, analog warmth, faded colors, light leaks, vintage photography | Film grain overlay, VHS tracking effect, polaroid shake, fade-in transitions, light leak animations | Photography portfolios, music/vinyl brands, vintage fashion, nostalgia marketing… | Modern tech, SaaS, healthcare, children's apps, corporate enterprise |
| Bauhaus (包豪斯) | bauhaus, geometric, constructivist, primary colors, hard shadow, bold, tactile, functional, poster, mechanical… | Hard offset shadows (4px 4px 0px black), mechanical press active:translate, no smooth hover — instant 0ms tran… | Mobile-first apps needing high personality, onboarding flows, branding-forward p… | Enterprise dashboards, accessibility-critical contexts (requires extra… |
| Material 3 Expressive (Mobile) | material 3 expressive, vibrant color, spring motion, adaptive components, flexible typography, contrasting sha… | Tonal elevation (overlay colors instead of strong shadows), pill-shaped buttons and chips (borderRadius 999)… | Android, Wear OS, and Pixel-aligned products using Material 3 components | Ultra-minimal brutalist brands, terminal/hacker aesthetics, monochrome… |
| Fluent 2 | fluent 2, microsoft, enterprise, calm, rounded, tokenized, cross-platform, copilot | Subtle depth, calm transitions, platform-adaptive motion | Microsoft 365, Windows, Copilot, and enterprise line-of-business tools | Products that should not inherit Microsoft platform conventions |
| Shopify Polaris | shopify polaris, merchant admin, commerce, checkout, web components, app home | Purposeful admin feedback and restrained transitions | Shopify admin apps, merchant tools, checkout, customer accounts, POS, and extens… | Generic marketing sites or products outside Shopify surfaces |
| Adobe Spectrum | adobe spectrum, creative tools, enterprise, content creation, tokenized, cross-platform | Layered depth and restrained professional motion | Creative tools, media workflows, document products, and Adobe-adjacent enterpris… | Consumer brands that do not need dense professional-tool conventions |

Notes: dashboard variants (Executive, Real-Time Monitoring, Drill-Down, Financial, etc.) are treated as sub-styles of **Data-Dense Dashboard**; mobile variants (Terminal CLI, Kinetic Brutalism, Flat Design Mobile…) map to their parent style (HUD/Sci-Fi, Brutalism, Flat Design). Avoid mixing flat and skeuomorphic styles arbitrarily; keep one style language across all pages.

## 2C. Font Pairings (74)
Load via Google Fonts with `font-display: swap`; preload only critical weights.

| Pairing | Heading | Body | Mood | Best for |
|---|---|---|---|---|
| Classic Elegant | Playfair Display | Inter | elegant, luxury, sophisticated, timeless, premium, editorial | Luxury brands, fashion, spa, beauty, editorial, magazines, high-end e-… |
| Modern Professional | Poppins | Open Sans | modern, professional, clean, corporate, friendly, approachable | SaaS, corporate sites, business apps, startups, professional services |
| Tech Startup | Space Grotesk | DM Sans | tech, startup, modern, innovative, bold, futuristic | Tech companies, startups, SaaS, developer tools, AI products |
| Editorial Classic | Cormorant Garamond | Libre Baskerville | editorial, classic, literary, traditional, refined, bookish | Publishing, blogs, news sites, literary magazines, book covers |
| Minimal Swiss | Inter | Inter | minimal, clean, swiss, functional, neutral, professional | Dashboards, admin panels, documentation, enterprise apps, design syste… |
| Playful Creative | Fredoka | Nunito | playful, friendly, fun, creative, warm, approachable | Children's apps, educational, gaming, creative tools, entertainment |
| Bold Statement | Bebas Neue | Source Sans 3 | bold, impactful, strong, dramatic, modern, headlines | Marketing sites, portfolios, agencies, event pages, sports |
| Wellness Calm | Lora | Raleway | calm, wellness, health, relaxing, natural, organic | Health apps, wellness, spa, meditation, yoga, organic brands |
| Developer Mono | JetBrains Mono | IBM Plex Sans | code, developer, technical, precise, functional, hacker | Developer tools, documentation, code editors, tech blogs, CLI apps |
| Retro Vintage | Abril Fatface | Merriweather | retro, vintage, nostalgic, dramatic, decorative, bold | Vintage brands, breweries, restaurants, creative portfolios, posters |
| Geometric Modern | Outfit | Work Sans | geometric, modern, clean, balanced, contemporary, versatile | General purpose, portfolios, agencies, modern brands, landing pages |
| Luxury Serif | Cormorant | Montserrat | luxury, high-end, fashion, elegant, refined, premium | Fashion brands, luxury e-commerce, jewelry, high-end services |
| Friendly SaaS | Plus Jakarta Sans | Plus Jakarta Sans | friendly, modern, saas, clean, approachable, professional | SaaS products, web apps, dashboards, B2B, productivity tools |
| News Editorial | Newsreader | Roboto | news, editorial, journalism, trustworthy, readable, informative | News sites, blogs, magazines, journalism, content-heavy sites |
| Handwritten Charm | Caveat | Quicksand | handwritten, personal, friendly, casual, warm, charming | Personal blogs, invitations, creative portfolios, lifestyle brands |
| Corporate Trust | Lexend | Source Sans 3 | corporate, trustworthy, accessible, readable, professional, clean | Enterprise, government, healthcare, finance, accessibility-focused |
| Brutalist Raw | Space Mono | Space Mono | brutalist, raw, technical, monospace, minimal, stark | Brutalist designs, developer portfolios, experimental, tech art |
| Fashion Forward | Syne | Manrope | fashion, avant-garde, creative, bold, artistic, edgy | Fashion brands, creative agencies, art galleries, design studios |
| Soft Rounded | Varela Round | Nunito Sans | soft, rounded, friendly, approachable, warm, gentle | Children's products, pet apps, friendly brands, wellness, soft UI |
| Premium Sans (DM Sans) | DM Sans | DM Sans | premium, modern, clean, sophisticated, versatile, balanced | Premium brands, modern agencies, SaaS, portfolios, startups |
| Vietnamese Friendly | Be Vietnam Pro | Noto Sans | vietnamese, international, readable, clean, multilingual, accessible | Vietnamese sites, multilingual apps, international products |
| Japanese Elegant | Noto Serif JP | Noto Sans JP | japanese, elegant, traditional, modern, multilingual, readable | Japanese sites, Japanese restaurants, cultural sites, anime/manga |
| Korean Modern | Noto Sans KR | Noto Sans KR | korean, modern, clean, professional, multilingual, readable | Korean sites, K-beauty, K-pop, Korean businesses, multilingual |
| Chinese Traditional | Noto Serif TC | Noto Sans TC | chinese, traditional, elegant, cultural, multilingual, readable | Traditional Chinese sites, cultural content, Taiwan/Hong Kong markets |
| Chinese Simplified | Noto Sans SC | Noto Sans SC | chinese, simplified, modern, professional, multilingual, readable | Simplified Chinese sites, mainland China market, business apps |
| Arabic Elegant | Noto Naskh Arabic | Noto Sans Arabic | arabic, elegant, traditional, cultural, RTL, readable | Arabic sites, Middle East market, Islamic content, bilingual sites |
| Thai Modern | Noto Sans Thai | Noto Sans Thai | thai, modern, readable, clean, multilingual, accessible | Thai sites, Southeast Asia, tourism, Thai restaurants |
| Hebrew Modern | Noto Sans Hebrew | Noto Sans Hebrew | hebrew, modern, RTL, clean, professional, readable | Hebrew sites, Israeli market, Jewish content, bilingual sites |
| Legal Professional | EB Garamond | Lato | legal, professional, traditional, trustworthy, formal, authoritative | Law firms, legal services, contracts, formal documents, government |
| Medical Clean | Figtree | Noto Sans | medical, clean, accessible, professional, healthcare, trustworthy | Healthcare, medical clinics, pharma, health apps, accessibility |
| Financial Trust | IBM Plex Sans | IBM Plex Sans | financial, trustworthy, professional, corporate, banking, serious | Banks, finance, insurance, investment, fintech, enterprise |
| Real Estate Luxury | Cinzel | Josefin Sans | real estate, luxury, elegant, sophisticated, property, premium | Real estate, luxury properties, architecture, interior design |
| Restaurant Menu | Playfair Display SC | Karla | restaurant, menu, culinary, elegant, foodie, hospitality | Restaurants, cafes, food blogs, culinary, hospitality |
| Art Deco | Poiret One | Didact Gothic | art deco, vintage, 1920s, elegant, decorative, gatsby | Vintage events, art deco themes, luxury hotels, classic cocktails |
| Magazine Style | Libre Bodoni | Public Sans | magazine, editorial, publishing, refined, journalism, print | Magazines, online publications, editorial content, journalism |
| Crypto/Web3 | Orbitron | Exo 2 | crypto, web3, futuristic, tech, blockchain, digital | Crypto platforms, NFT, blockchain, web3, futuristic tech |
| Gaming Bold | Russo One | Chakra Petch | gaming, bold, action, esports, competitive, energetic | Gaming, esports, action games, competitive sports, entertainment |
| Indie/Craft | Amatic SC | Cabin | indie, craft, handmade, artisan, organic, creative | Craft brands, indie products, artisan, handmade, organic products |
| Startup Bold (Outfit + Rubik) | Outfit | Rubik | startup, bold, modern, innovative, confident, dynamic | Startups, pitch decks, product launches, bold brands |
| E-commerce Clean | Rubik | Nunito Sans | ecommerce, clean, shopping, product, retail, conversion | E-commerce, online stores, product pages, retail, shopping |
| Academic/Research | Crimson Pro | Atkinson Hyperlegible | academic, research, scholarly, accessible, readable, educational | Universities, research papers, academic journals, educational |
| Dashboard Data | Fira Code | Fira Sans | dashboard, data, analytics, code, technical, precise | Dashboards, analytics, data visualization, admin panels |
| Music/Entertainment | Righteous | Poppins | music, entertainment, fun, energetic, bold, performance | Music platforms, entertainment, events, festivals, performers |
| Minimalist Portfolio | Archivo | Space Grotesk | minimal, portfolio, designer, creative, clean, artistic | Design portfolios, creative professionals, minimalist brands |
| Kids/Education | Baloo 2 | Comic Neue | kids, education, playful, friendly, colorful, learning | Children's apps, educational games, kid-friendly content |
| Wedding/Romance | Great Vibes | Cormorant Infant | wedding, romance, elegant, script, invitation, feminine | Wedding sites, invitations, romantic brands, bridal |
| Science/Tech | Exo | Roboto Mono | science, technology, research, data, futuristic, precise | Science, research, tech documentation, data-heavy sites |
| Accessibility First | Atkinson Hyperlegible | Atkinson Hyperlegible | accessible, readable, inclusive, WCAG, dyslexia-friendly, clear | Accessibility-critical sites, government, healthcare, inclusive design |
| Sports/Fitness | Barlow Condensed | Barlow | sports, fitness, athletic, energetic, condensed, action | Sports, fitness, gyms, athletic brands, competition |
| Luxury Minimalist | Bodoni Moda | Jost | luxury, minimalist, high-end, sophisticated, refined, premium | Luxury minimalist brands, high-end fashion, premium products |
| Tech/HUD Mono | Share Tech Mono | Fira Code | tech, futuristic, hud, sci-fi, data, monospaced, precise | Sci-fi interfaces, developer tools, cybersecurity, dashboards |
| Pixel Retro | Press Start 2P | VT323 | pixel, retro, gaming, 8-bit, nostalgic, arcade | Pixel art games, retro websites, creative portfolios |
| Neubrutalist Bold | Lexend Mega | Public Sans | bold, neubrutalist, loud, strong, geometric, quirky | Neubrutalist designs, Gen Z brands, bold marketing |
| Academic/Archival | EB Garamond | Crimson Text | academic, old-school, university, research, serious, traditional | University sites, archives, research papers, history |
| Spatial Clear | Inter | Inter | spatial, legible, glass, system, clean, neutral | Spatial computing, AR/VR, glassmorphism interfaces |
| Kinetic Motion | Syncopate | Space Mono | kinetic, motion, futuristic, speed, wide, tech | Music festivals, automotive, high-energy brands |
| Gen Z Brutal | Anton | Epilogue | brutal, loud, shouty, meme, internet, bold | Gen Z marketing, streetwear, viral campaigns |
| Bauhaus Geometric | Outfit | Outfit | bauhaus, geometric, constructivist, bold, uppercase, architectural, me… | Bauhaus mobile apps, bold editorial mobile, design-forward branding ap… |
| Minimalist Monochrome Editorial | Playfair Display | Source Serif 4 | monochrome, editorial, austere, typographic, pocket manifesto, luxury… | Luxury fashion mobile apps, editorial publications, digital exhibition… |
| Modern Dark Cinema (Inter System) | Inter | Inter | dark, cinematic, technical, precision, clean, premium, developer, prof… | Developer tools, fintech/trading, AI dashboards, streaming platforms… |
| SaaS Mobile Boutique (Calistoga + Inter) | Calistoga | Inter | saas, boutique, electric, warm, editorial, bold, premium, fintech, bus… | B2B SaaS mobile, fintech apps, analytics dashboards, marketing tools… |
| Terminal CLI Monospace | JetBrains Mono | JetBrains Mono | terminal, cli, hacker, monospace, matrix, developer, retro-future, com… | Developer tools, Web3/blockchain apps, hacker aesthetic, sci-fi games… |
| Kinetic Brutalism (Space Grotesk) | Space Grotesk | Space Grotesk | kinetic, brutalist, aggressive, uppercase, oversized, display, motion… | Music/culture apps, sports platforms, brand flagship mobile, performan… |
| Flat Design Mobile (System Bold) | Inter | Inter | flat, clean, system, bold, geometric, cross-platform, icon, poster, mi… | Cross-platform apps, dashboards, system UI, onboarding, marketing page… |
| Material You MD3 (Roboto System) | Roboto | Roboto | material design 3, md3, android, google, tonal, friendly, rounded, acc… | Android apps, cross-platform tools, productivity software, data-heavy… |
| Neo Brutalism Mobile (Space Grotesk Bold) | Space Grotesk | Space Grotesk | neo brutalism, pop art, loud, bold, heavy, stickers, mechanical, high… | Creative tools, Gen-Z marketing, e-commerce for youth culture, content… |
| Bold Typography Mobile (Inter Poster) | Inter | Playfair Display | bold typography, editorial, poster, near-black, vermillion, luxury, ty… | Creative brand flagships, reading platforms, event apps, flash pages… |
| Academia Mobile (Cormorant + Crimson + Cinzel) | Cormorant Garamond | Crimson Pro | academia, library, mahogany, parchment, brass, scholarly, prestige, an… | Knowledge management apps, scholarly reading tools, personal brand por… |
| Cyberpunk Mobile (Orbitron + JetBrains Mono) | Orbitron | JetBrains Mono | cyberpunk, neon, glitch, hud, sci-fi, dark, matrix green, magenta, cha… | Gaming companion apps, fintech/crypto, data visualization, dark brand… |
| Web3 Bitcoin DeFi (Space Grotesk + Inter + Mono) | Space Grotesk | Inter | web3, bitcoin, defi, digital gold, fintech, crypto, trustless, lumines… | DeFi protocols and wallets, NFT platforms, metaverse social apps, high… |
| Claymorphism Mobile (Nunito + DM Sans) | Nunito | DM Sans | claymorphism, clay, rounded, playful, candy, bubbly, soft, 3d, childre… | Children education apps, teen social, brand mascot apps, creative tool… |
| Enterprise SaaS Mobile (Plus Jakarta Sans) | Plus Jakarta Sans | Plus Jakarta Sans | enterprise, saas, b2b, professional, indigo, modern, approachable, leg… | B2B SaaS apps, productivity tools, government and finance mobile apps… |
| Sketch Hand-Drawn Mobile (Kalam + Patrick Hand) | Kalam | Patrick Hand | sketch, hand-drawn, handwriting, human, imperfect, organic, paper, kal… | Journaling apps, prototype tools, children's picturebook apps, creativ… |
| Neumorphism Mobile (Plus Jakarta Sans + System) | Plus Jakarta Sans | Plus Jakarta Sans | neumorphism, soft ui, monochromatic, cool grey, minimal, physical, dep… | Smart home controls, minimal tools, aesthetic dashboards, health monit… |

## 2D. Landing / Page Patterns (34)

| Pattern | Section order | Primary CTA placement | Conversion notes |
|---|---|---|---|
| Hero + Features + CTA | Hero with headline/image > Value prop > Key features (3-5) > CTA section > Footer | Hero (sticky) + Bottom | Deep CTA placement. For CTA label text, verify at least 4.5:1 against the button fill; use 7:1 only when the p… |
| Hero + Testimonials + CTA | Hero > Problem statement > Solution overview > Testimonials carousel > CTA | Hero (sticky) + Post-testimonials | Social proof before CTA. Use a concise set of verified testimonials with photo, name, and role. CTA after soci… |
| Product Demo + Features | Hero > Product video/mockup (center) > Feature breakdown per section > Comparison (optional) > CTA | Video center + CTA right/bottom | Use an interactive demo only when it explains value better than static media. Provide captions, transcript, vi… |
| Minimal Single Column | Hero headline > Short description > Benefit bullets (3 max) > CTA > Footer | Center, large CTA button | Single CTA focus. Large typography. Lots of whitespace. No nav clutter. Mobile-first. |
| Funnel (3-Step Conversion) | Hero > Step 1 (problem) > Step 2 (solution) > Step 3 (action) > CTA progression | Each step: mini-CTA. Final: main CTA | Progressive disclosure. Show only essential info per step. Use progress indicators. Multiple CTAs. |
| Comparison Table + CTA | Hero > Problem intro > Comparison table (product vs competitors) > Pricing (optional) > CTA | Table: Right column. CTA: Below table | Use comparison to show unique value. Highlight your product row. Include 'free trial' in pricing row. |
| Lead Magnet + Form | Hero (benefit headline) > Lead magnet preview (ebook cover, checklist, etc) > Form (minimal fields) > CTA submit | Form CTA: Submit button | Ask only for information necessary to deliver the lead magnet. Preview its value and show submission progress. |
| Pricing Page + CTA | Hero (pricing headline) > Price comparison cards > Feature comparison table > FAQ section > Final CTA | Each card: CTA button. Sticky CTA in nav | Highlight the plan that matches the intended audience and show actual annual savings transparently. Use FAQs t… |
| Video-First Hero | Hero with video background > Key features overlay > Benefits section > CTA | Overlay on video (center/bottom) + Botto… | Use video only when it demonstrates value better than static media. Add captions for accessibility. Compress v… |
| Scroll-Triggered Storytelling | Intro hook > Chapter 1 (problem) > Chapter 2 (journey) > Chapter 3 (solution) > Climax CTA | End of each chapter (mini) + Final clima… | Keep the narrative understandable without scroll-driven effects. Use progress indicator. Mobile: simplify anim… |
| AI Personalization Landing | Dynamic hero (personalized) > Relevant features > Tailored testimonials > Smart CTA | Context-aware placement based on user se… | Validate personalization with consent-aware product analytics. Requires analytics integration. Fallback for ne… |
| Waitlist/Coming Soon | Hero with countdown > Product teaser/preview > Email capture form > Social proof (waitlist count) | Email form prominent (above fold) + Stic… | Explain early-access benefits without fabricated scarcity. Show a waitlist count only when it is current, veri… |
| Comparison Table Focus | Hero (problem statement) > Comparison matrix (you vs competitors) > Feature deep-dive > Winner CTA | After comparison table (highlighted row)… | Show value vs competitors. Measure comparison performance with product-specific analytics. Be factual. Include… |
| Pricing-Focused Landing | Hero (value proposition) > Pricing cards (3 tiers) > Feature comparison > FAQ > Final CTA | Each pricing card + Sticky CTA in nav +… | Show actual monthly and annual totals and savings transparently. Explain plan differences and address objectio… |
| App Store Style Landing | Hero with device mockup > Screenshots carousel > Features with icons > Reviews/ratings > Download CTAs | Download buttons prominent (App Store +… | Show real screenshots and only current verified ratings. Provide platform-specific CTAs, buttons and keyboard… |
| FAQ/Documentation Landing | Hero with search bar > Popular categories > FAQ accordion > Contact/support CTA | Search bar prominent + Contact CTA for u… | Reduce support tickets. Track search analytics. Show related articles. Contact escalation path. |
| Immersive/Interactive Experience | Full-screen interactive element > Guided product tour > Key benefits revealed > CTA after completion | After interaction complete + Skip option… | Measure engagement for the specific audience and device mix. Performance trade-off. Provide skip option. Mobil… |
| Event/Conference Landing | Hero (date/location/countdown) > Speakers grid > Agenda/schedule > Sponsors > Register CTA | Register CTA sticky + After speakers + B… | Early bird pricing with deadline. Social proof (past attendees). Speaker credibility. Multi-ticket discounts.… |
| Product Review/Ratings Focused | Hero (product + aggregate rating) > Rating breakdown > Individual reviews > Buy/CTA | After reviews summary + Buy button along… | User-generated content builds trust. Show verified purchases. Filter by rating. Respond to negative reviews. |
| Community/Forum Landing | Hero (community value prop) > Popular topics/categories > Active members showcase > Join CTA | Join button prominent + After member sho… | Preview real community value and simplify onboarding. Show member and activity counts only when current, verif… |
| Before-After Transformation | Hero (problem state) > Transformation slider/comparison > How it works > Results CTA | After transformation reveal + Bottom | Visual proof of value. Measure the outcome with product-specific analytics. Real results. Specific metrics. Gu… |
| Marketplace / Directory | Hero (Search focused) > Categories > Featured Listings > Trust/Safety > CTA (Become a host/seller) | Hero Search Bar + Navbar 'List your item… | Search is the primary CTA. Reduce friction with useful suggestions. For featured-listing carousels, provide pr… |
| Newsletter / Content First | Hero (Value Prop + Form) > Recent Issues/Archives > Social Proof (Subscriber count) > About Author | Hero inline form + Sticky header form | Keep the form to the fields actually required and link to a sample issue. Show a subscriber count only when it… |
| Webinar Registration | Hero (Topic + Timer + Form) > What you'll learn > Speaker Bio > Urgency/Bonuses > Form (again) | Hero (Right side form) + Bottom anchor | State the event time and timezone in text. Claim limited seats or live status only when current capacity or st… |
| Enterprise Gateway | Hero (Video/Mission) > Solutions by Industry > Solutions by Role > Client Logos > Contact Sales | Contact Sales (Primary) + Login (Seconda… | Path selection (I am a...). Mega menu navigation. Trust signals prominent. Provide pause/stop for video and ro… |
| Portfolio Grid | Hero (Name/Role) > Project Grid (Masonry) > About/Philosophy > Contact | Project Card Hover + Footer Contact | Visuals first. Filter by category. Fast loading essential. |
| Horizontal Scroll Journey | Intro (Vertical) > The Journey (Horizontal Track) > Detail Reveal > Vertical Footer | Floating Sticky CTA or End of Horizontal… | Immersive product discovery. High engagement. Keep navigation visible. Preserve a normal vertical navigation p… |
| Bento Grid Showcase | Hero > Bento Grid (Key Features) > Detail Cards > Tech Specs > CTA | Floating Action Button or Bottom of Grid | Scannable value props. High information density without clutter. Mobile stack. Keep cards usable without hover… |
| Interactive 3D Configurator | Hero (Configurator) > Feature Highlight (synced) > Price/Specs > Purchase | Inside Configurator UI + Sticky Bottom B… | Let users inspect product details before purchase. Provide buttons and keyboard controls for rotate/zoom plus… |
| AI-Driven Dynamic Landing | Prompt/Input Hero > Generated Result Preview > How it Works > Value Prop | Input Field (Hero) + 'Try it' Buttons | Immediate value demonstration. 'Show, don't tell'. Low friction start. Disable typing, shimmer, and morphing e… |
| Feature-Rich Showcase | Hero (value prop) > Feature grid/cards (4-6) > Use cases or benefits > Social proof or logos > CTA | Hero (sticky) + After features + Bottom | Clear feature hierarchy. One key message per card. Strong CTA repetition. |
| Hero-Centric Design | Full-bleed Hero (headline + visual) > Single value prop strip > Key benefit or proof > Primary CTA | Hero dominant (center/bottom) + Sticky n… | One primary CTA. Let the hero dominate the initial viewport without hiding the next content cue. Use a static… |
| Trust & Authority + Conversion | Hero (mission/credibility) > Proof (logos, certs, stats) > Solution overview > Clear CTA path | Contact Sales / Get Quote (primary) + Na… | Security badges. Case studies. Transparent pricing. Low-friction form. Provide pause/stop and stop the logo ca… |
| Real-Time / Operations Landing | Hero (product + live preview or status) > Key metrics/indicators > How it works > CTA (Start trial / Contact) | Primary CTA in nav + After metrics | Offer a demo or sandbox and show trust signals. Label telemetry as live only when backed by a current source… |

## 2E. Chart Selection (25 data types)

| Data type | Best chart | Alternatives | Do not use when | Accessibility fallback | Libraries |
|---|---|---|---|---|---|
| Trend Over Time | Line Chart | Area Chart, Smooth Area | Fewer than 4 data points (use stat card); more than 6 series (visual noise); no time dimen… | Visible data table plus concise trend summary. Keyboard: focus reveals hover values; +/- buttons zoo… | Chart.js, Recharts, ApexCharts |
| Compare Categories | Bar Chart (Horizontal or Vertical) | Column Chart, Grouped Bar | Categories > 15 (use table or search); data has time dimension (use line); showing proport… | Visible sortable data table plus concise comparison summary. Keyboard: focus reveals hover values; f… | Chart.js, Recharts, D3.js |
| Part-to-Whole | Pie Chart or Donut | Stacked Bar, Waffle Chart | Categories > 5; slice differences < 5% (visually indistinguishable); user needs precise va… | Percentage data table and concise part-to-whole summary; offer a stacked bar view. Keyboard: focus r… | Chart.js, Recharts, D3.js |
| Correlation / Distribution | Scatter Plot or Bubble Chart | Heat Map, Matrix | Variables are categorical (use grouped bar); fewer than 20 points (patterns aren't meaning… | Visible data table plus correlation summary. Keyboard: focus reveals point values; labeled start/end… | D3.js, Plotly, Recharts |
| Heatmap / Intensity | Heat Map or Choropleth | Grid Heat Map, Bubble Heat | Fewer than 20 cells (use bar); user needs to read exact values; colorblind users without p… | Grid data table plus intensity summary. Keyboard: focus reveals cell values; +/- buttons zoom; Reset… | D3.js, Plotly, ApexCharts |
| Geographic Data | Choropleth Map or Bubble Map | Geographic Heat Map | Regions have very different sizes making visual comparison misleading (use bar); mobile-pr… | Sortable region table plus geographic summary. Keyboard: arrow keys or labeled pan buttons move the… | D3.js, Mapbox, Leaflet |
| Funnel / Flow | Funnel Chart or Sankey | Waterfall (for flows) | Stages aren't sequential; values don't decrease monotonically (use bar); fewer than 3 stag… | Linear stage table/list plus conversion summary. Keyboard: focus reveals stage values; Enter/Space d… | D3.js, Recharts, Custom SVG |
| Performance vs Target | Gauge Chart or Bullet Chart | Dial, Thermometer | No target or benchmark exists; comparing multiple KPIs at once (use bullet chart grid) | Visible KPI/target text and a one-row data table plus status summary. Keyboard: focus reveals the sa… | D3.js, ApexCharts, Custom SVG |
| Time-Series Forecast | Line with Confidence Band | Ribbon Chart | No historical baseline; prediction confidence is too low to be useful; audience is not dat… | Visible forecast table plus uncertainty summary. Keyboard: focus reveals hover values; buttons toggl… | Chart.js, ApexCharts, Plotly |
| Anomaly Detection | Line Chart with Highlights | Scatter with Alert | Anomalies are predefined categories (use bar with highlight); real-time context without a… | Anomaly event list/table plus narrative alert summary. Keyboard: focus reveals point details; alerts… | D3.js, Plotly, ApexCharts |
| Hierarchical / Nested Data | Treemap | Sunburst, Nested Donut, Icicle | Hierarchy depth > 3 levels (too complex to read); user needs to compare sibling values pre… | Collapsible tree table plus hierarchy summary; treemap remains supplementary. Keyboard: focus reveal… | D3.js, Recharts, ApexCharts |
| Flow / Process Data | Sankey Diagram | Alluvial, Chord Diagram | Flow directions form loops (use network graph); fewer than 3 source-target pairs; mobile-p… | Source-to-target flow table plus flow summary. Keyboard: focus reveals hover values; Enter/Space dri… | D3.js (d3-sankey), Plotly |
| Cumulative Changes | Waterfall Chart | Stacked Bar, Cascade | Changes are not additive; more than 12 bars (readability breaks); audience expects a simpl… | Running-total table plus variance summary. Keyboard: focus reveals the same value as hover. | ApexCharts, Highcharts, Plotly |
| Multi-Variable Comparison | Radar / Spider Chart | Parallel Coordinates, Grouped Bar | Axes > 8 (unreadable); values need precise comparison (use grouped bar); audience unfamili… | Raw data table and grouped-bar alternative plus comparison summary. Keyboard: focus reveals values… | Chart.js, Recharts, ApexCharts |
| Stock / Trading OHLC | Candlestick Chart | OHLC Bar, Heikin-Ashi | Non-financial audience; no OHLC data available (use line chart); accessibility-first conte… | Sortable OHLC table plus daily-change summary. Keyboard: focus reveals candle values; +/- buttons zo… | Lightweight Charts (TradingView), ApexCh… |
| Relationship / Connection Data | Network Graph | Hierarchical Tree, Adjacency Matrix | Node count > 500 without clustering pre-applied; user needs precise connection counts; mob… | Adjacency list/table and relationship summary; tree view when applicable. Keyboard: focus reveals no… | D3.js (d3-force), Vis.js, Cytoscape.js |
| Distribution / Statistical | Box Plot | Violin Plot, Beeswarm | Fewer than 20 data points per group (distribution is not meaningful); audience unfamiliar… | Statistics table plus distribution summary. Keyboard: focus reveals the same statistics as hover. | Plotly, D3.js, Chart.js (plugin) |
| Performance vs Target (Compact) | Bullet Chart | Gauge, Progress Bar | Single KPI with emphasis (use gauge); data has no defined target range; fewer than 3 KPIs | Visible KPI/target table plus status summary. Keyboard: focus reveals the same detail as hover. | D3.js, Plotly, Custom SVG |
| Proportional / Percentage | Waffle Chart | Pictogram, Stacked Bar 100% | More than 5 categories (use stacked bar); exact values matter over visual proportion; very… | Percentage table/list plus part-to-whole summary. Keyboard: focus reveals each cell value. | D3.js, React-Waffle, Custom CSS Grid |
| Hierarchical Proportional | Sunburst Chart | Treemap, Icicle, Circle Packing | More than 3 hierarchy levels (outer rings become unreadable); precision matters over overv… | Collapsible indented list/table plus hierarchy summary and breadcrumbs. Keyboard: Enter/Space drills… | D3.js (d3-hierarchy), Recharts, ApexChar… |
| Root Cause Analysis | Decomposition Tree | Decision Tree, Flow Chart | No clear parent-child causal relationship; audience expects a summary rather than explorat… | Expandable tree table plus root-cause summary. Keyboard: Enter/Space drills and expands; Back collap… | Power BI (native), React-Flow, Custom D3… |
| 3D Spatial Data | 3D Scatter / Surface Plot | Volumetric Rendering, Point Cloud | 2D projection conveys the same insight; mobile context; accessibility-required environment… | Mandatory 2D projection, data table, and spatial summary. Keyboard: rotate/pan buttons and +/- zoom… | Three.js, Deck.gl, Plotly 3D |
| Real-Time Streaming | Streaming Area Chart | Ticker Tape, Moving Gauge | Update frequency < 1/min (use periodic-refresh line chart); flashing content without reduc… | Streaming data table plus current-value/trend summary. Keyboard: Pause/Resume button controls update… | Smoothed D3.js, CanvasJS |
| Sentiment / Emotion | Word Cloud with Sentiment | Sentiment Arc, Radar Chart | Precise values matter (word size is inherently imprecise); screen-reader context; corpus <… | Sortable term table/list plus sentiment summary; word cloud is supplementary. Keyboard: focus reveal… | D3-cloud, Highcharts, Nivo |
| Process Mining | Process Map / Graph | Directed Acyclic Graph (DAG), Petri Net | No event log data available; audience expects a static flowchart (use diagram tool); node… | Path summary table plus bottleneck narrative. Keyboard: Move buttons replace drag; focus reveals nod… | React-Flow, Cytoscape.js, Recharts |

Always: supply a data-table or text-summary alternative, don't rely on color alone, label axes with units, respect reduced-motion for chart entrances.

## 2F. GSAP Motion Presets (17)
Always gate non-essential motion behind `matchMedia('(prefers-reduced-motion: reduce)')` and render the final state immediately when it matches. Animate `transform`/`opacity` only.

| Category | Tier | Duration | Easing | Snippet | Don't |
|---|---|---|---|---|---|
| Hover Micro-interaction | Subtle | 150-200ms | power1.out | `gsap.to(el, { y: -1, opacity: 0.9, duration: 0.15, ease: 'power1.out' });` | Don't animate layout-affecting props (width/height/margin) on hover |
| Hover Micro-interaction | Standard | 200-300ms | power2.out | `gsap.to(el, { y: -4, scale: 1.02, boxShadow: '0 12px 24px rgba(0,0,0,0.12)', duration: 0.25, ease: 'power2.out' });` | Don't leave the hover state stuck if the pointer leaves fast; always attach the… |
| Hover Micro-interaction | Complex | 300-500ms | elastic.out(1,0.4) | `const onPointerMove = (e) => { const r = el.getBoundingClientRect(); xTo((e.clientX - r.left - r.width / 2) * 0.3); yTo((e.clientY - r.top - r.height…` | Don't apply magnetic effect to more than 1-2 focal elements per screen; it becom… |
| Scroll Reveal | Subtle | 300-400ms | power1.out | `gsap.from(el, { opacity: 0, y: 12, duration: 0.35, ease: 'power1.out', scrollTrigger: { trigger: el, start: 'top 90%', toggleActions: 'play none none…` | Don't reveal below-the-fold content needed for SEO/crawlers as invisible-by-defa… |
| Scroll Reveal | Standard | 400-600ms | power2.out | `gsap.from(el.children, { opacity: 0, y: 24, duration: 0.5, stagger: 0.08, ease: 'power2.out', scrollTrigger: { trigger: el, start: 'top 85%' } });` | Don't stagger more than ~8 children; beyond that the last items feel laggy |
| Scroll Reveal | Complex | tied to scroll position | none (scrub-driven) | `gsap.timeline({ scrollTrigger: { trigger: section, start: 'top top', end: '+=150%', scrub: 1, pin: true } }).from('.headline', { opacity: 0, y: 40 }).…` | Don't pin more than 1-2 sections per page; excessive pinning fights native scrol… |
| Stagger List | Subtle | 250-350ms | power1.out | `gsap.from('.list-item', { opacity: 0, y: 8, duration: 0.3, stagger: 0.03 });` | Don't stagger by more than 0.1s per item on long lists; total reveal time become… |
| Stagger List | Standard | 300-450ms | back.out(1.4) | `gsap.from('.grid-item', { opacity: 0, scale: 0.92, y: 16, duration: 0.4, stagger: { each: 0.06, from: 'start', grid: 'auto' }, ease: 'back.out(1.4)' }…` | Don't use back.out on dense data tables; the overshoot reads as sloppy on inform… |
| Stagger List | Complex | 400-700ms | expo.out | `const split = new SplitText(headline, { type: 'chars' }); gsap.from(split.chars, { opacity: 0, y: 20, rotateX: -40, duration: 0.6, stagger: 0.015, eas…` | Don't split-animate long paragraphs; reserve for short headlines (under ~8 words… |
| Page Transition | Subtle | 200-300ms | power1.inOut | `gsap.to(main, { opacity: 0, duration: 0.2, onComplete: () => { navigate(); gsap.fromTo(main, { opacity: 0 }, { opacity: 1, duration: 0.2 }); } });` | Don't block navigation on animation; cap exit duration at ~250ms so the app neve… |
| Page Transition | Standard | 400-600ms | power2.inOut | `const tl = gsap.timeline(); tl.to('.transition-overlay', { yPercent: 0, duration: 0.4, ease: 'power2.inOut' }).call(navigate).to('.transition-overlay'…` | Don't tie the overlay's reveal directly to data-fetch completion without a max-w… |
| Page Transition | Complex | 500-800ms | expo.inOut | `const state = Flip.getState('.hero-image'); navigate(); Flip.from(state, { duration: 0.6, ease: 'expo.inOut', absolute: true, zIndex: 100 });` | Don't use shared-element transitions across more than one element pair per navig… |
| Parallax Scroll | Subtle | tied to scroll position | linear (scrub) | `gsap.to('.bg-layer', { yPercent: 10, ease: 'none', scrollTrigger: { trigger: section, scrub: true } });` | Don't parallax body copy; it hurts reading comfort and can trigger motion sickne… |
| Parallax Scroll | Standard | tied to scroll position | linear (scrub) | `gsap.utils.toArray('.parallax-layer').forEach((layer, i) => { gsap.to(layer, { yPercent: (i + 1) * -8, ease: 'none', scrollTrigger: { trigger: layer.p…` | Don't let parallax layers overflow their container; clip with overflow: hidden o… |
| Loading / Skeleton | Subtle | 1200-1600ms loop | sine.inOut | `const tween = gsap.to('.skeleton', { backgroundPosition: '200% 0', duration: 1.4, ease: 'sine.inOut', repeat: -1 }); return () => tween.kill();` | Don't run more than one shimmer loop per skeleton group; sync them under one tim… |
| Loading / Skeleton | Standard | 800-1200ms loop | power1.inOut | `const tl = gsap.timeline({ repeat: -1 }).to('.loader-dot', { y: -8, duration: 0.4, stagger: { each: 0.15, yoyo: true, repeat: 1 } }); const onVisibili…` | Don't use elaborate loaders for sub-300ms waits; they flash and feel worse than… |
| Carousel / Auto-Rotation | Standard | user-controlled or stopped | none | `const reduced = matchMedia('(prefers-reduced-motion: reduce)'); let timer; let onscreen = true; const stop = () => { clearInterval(timer); timer = und…` | Don't auto-advance without a visible stop control or continue while focus is ins… |

---

# Part 3 — Stack Guidance

Supported stacks: `react`, `nextjs`, `vue`, `svelte`, `astro`, `nuxtjs`, `nuxt-ui`, `angular`, `laravel`, `swiftui`, `react-native`, `flutter`, `jetpack-compose`, `html-tailwind`, `shadcn`, `threejs`, `javafx`, `wpf`, `winui`, `avalonia`, `uno`, `uwp`.

General rules for any stack:
- Detect the stack from project files; never default silently.
- Prefer the stack's native/system components (SwiftUI controls, Material components, shadcn/ui primitives, Nuxt UI, etc.) over fully custom ones, customizing only for brand needs.
- Web (React/Next/Vue/Svelte/Astro/Nuxt/Angular/Laravel/html-tailwind): semantic HTML first; tokens as CSS variables or Tailwind theme; `min-h-dvh` over `100vh`; `touch-action: manipulation`; split bundles by route; reserve space for async content (CLS); virtualize lists of 50+ items; debounce/throttle scroll/resize/input.
- React/Next specifics: lazy-load non-hero components with dynamic import/Suspense; avoid re-render storms in lists (memoize row components, stable keys); stream with Suspense boundaries; keep client components small.
- Tailwind: use semantic token names mapped to CSS variables (`bg-background text-foreground`), a consistent spacing scale, `overflow-wrap:anywhere` on user content, `text-balance` on short headings, and `tabular-nums` for data columns.
- Native (SwiftUI / Jetpack Compose / React Native / Flutter): follow platform conventions (iOS HIG tab bar vs Material top app bar), respect safe areas, support Dynamic Type / font scaling, use platform haptics and gestures consistently, 44pt (iOS) / 48dp (Android) targets.
- Desktop (WPF / WinUI / UWP / Avalonia / Uno / JavaFX): keyboard-first operation, visible focus, adaptive layouts, high-DPI assets, theme resources instead of hardcoded colors.
- Three.js: provide a non-WebGL fallback and reduced-motion path, cap pixel ratio, pause rendering off-screen, and keep text/UI in the DOM for accessibility.

---

# Part 4 — App UI Polish Rules & Pre-Delivery Checklist

Scope: targets native/mobile app UI (iOS/Android/React Native/Flutter). For web/desktop interaction patterns rely on Part 1; the tables assume touch targets, safe areas and platform gestures that do not apply 1:1 to desktop web. These are frequently overlooked issues that make UI look unprofessional.

### Icons & Visual Elements

| Rule | Standard | Avoid | Why It Matters |
|------|----------|--------|----------------|
| **No Emoji as Structural Icons** | Use vector-based icons (e.g., Lucide, react-native-vector-icons, @expo/vector-icons). | Using emojis (🎨 🚀 ⚙️) for navigation, settings, or system controls. | Emojis are font-dependent, inconsistent across platforms, and cannot be controlled via design tokens. |
| **Vector-Only Assets** | Use SVG or platform vector icons that scale cleanly and support theming. | Raster PNG icons that blur or pixelate. | Ensures scalability, crisp rendering, and dark/light mode adaptability. |
| **Contextual Semantics** | Choose semantics from use, not glyph: hide decorative icons beside visible text from the accessibility tree; give meaningful standalone icons a text alternative; give icon controls an accessible name and expose selected/pressed/expanded state when applicable. | Treating one icon name as permanently decorative, meaningful, or interactive. | The same glyph can serve different purposes in different components. |
| **Stable Interaction States** | Use color, opacity, or elevation transitions for press states without changing layout bounds. | Layout-shifting transforms that move surrounding content or trigger visual jitter. | Prevents unstable interactions and preserves smooth motion/perceived quality on mobile. |
| **Correct Brand Logos** | Use official brand assets and follow their usage guidelines (spacing, color, clear space). | Guessing logo paths, recoloring unofficially, or modifying proportions. | Prevents brand misuse and ensures legal/platform compliance. |
| **Consistent Icon Sizing** | Define icon sizes as design tokens (e.g., icon-sm, icon-md = 24pt, icon-lg). | Mixing arbitrary values like 20pt / 24pt / 28pt randomly. | Maintains rhythm and visual hierarchy across the interface. |
| **Stroke Consistency** | Use a consistent stroke width within the same visual layer (e.g., 1.5px or 2px). | Mixing thick and thin stroke styles arbitrarily. | Inconsistent strokes reduce perceived polish and cohesion. |
| **Filled vs Outline Discipline** | Use one icon style per hierarchy level. | Mixing filled and outline icons at the same hierarchy level. | Maintains semantic clarity and stylistic coherence. |
| **Touch Target Minimum** | Use at least 44pt on iOS and 48dp on Android; expand the hit area when the visual icon is smaller. | Small icons without expanded tap area, or one unit reused across platforms. | Matches platform-specific target guidance. |
| **Icon Alignment** | Align icons to text baseline and maintain consistent padding. | Misaligned icons or inconsistent spacing around them. | Prevents subtle visual imbalance that reduces perceived quality. |
| **Icon Contrast** | Meaningful icons and control boundaries need at least 3:1 against adjacent colors; decorative icons must not carry information. | Low-contrast icons that carry meaning or state. | Applies the non-text contrast role instead of a text-size rule. |

### Interaction (App)

| Rule | Do | Don't |
|------|----|----- |
| **Tap feedback** | Provide clear pressed feedback (ripple/opacity/elevation) within 80-150ms | No visual response on tap |
| **Animation timing** | Use shared tokens chosen for distance, complexity, platform, and user context | One duration/easing copied to every transition |
| **Accessibility focus** | Ensure screen reader focus order matches visual order and labels are descriptive | Unlabeled controls or confusing focus traversal |
| **Disabled state clarity** | Use disabled semantics (`disabled`/native disabled props), reduced emphasis, and no tap action | Controls that look tappable but do nothing |
| **Touch target minimum** | Keep tap areas >=44x44pt (iOS) or >=48x48dp (Android), expand hit area when icon is smaller | Tiny tap targets or icon-only hit areas without padding |
| **Gesture conflict prevention** | Keep one primary gesture per region and avoid nested tap/drag conflicts | Overlapping gestures causing accidental actions |
| **Semantic native controls** | Prefer native interactive primitives (`Button`, `Pressable`, platform equivalents) with proper accessibility roles | Generic containers used as primary controls without semantics |

### Light/Dark Mode Contrast

| Rule | Do | Don't |
|------|----|----- |
| **Surface readability (light)** | Keep cards/surfaces clearly separated from background with sufficient opacity/elevation | Overly transparent surfaces that blur hierarchy |
| **Text contrast (light)** | Maintain body text contrast >=4.5:1 against light surfaces | Low-contrast gray body text |
| **Text contrast (dark)** | Maintain normal text contrast >=4.5:1 on dark surfaces; 3:1 is only for large text or non-text UI | Muted normal text that falls below the text threshold |
| **Border and divider visibility** | Ensure separators are visible in both themes (not just light mode) | Theme-specific borders disappearing in one mode |
| **State contrast parity** | Keep pressed/focused/disabled states equally distinguishable in light and dark themes | Defining interaction states for one theme only |
| **Token-driven theming** | Use semantic color tokens mapped per theme across app surfaces/text/icons | Hardcoded per-screen hex values |
| **Scrim and modal legibility** | Measure the composed result and use a scrim strong enough to isolate foreground content | Reusing one opacity without checking the actual background |

### Layout & Spacing

| Rule | Do | Don't |
|------|----|----- |
| **Safe-area compliance** | Respect top/bottom safe areas for all fixed headers, tab bars, and CTA bars | Placing fixed UI under notch, status bar, or gesture area |
| **System bar clearance** | Add spacing for status/navigation bars and gesture home indicator | Let tappable content collide with OS chrome |
| **Consistent content width** | Keep predictable content width per device class (phone/tablet) | Mixing arbitrary widths between screens |
| **8dp spacing rhythm** | Use a consistent 4/8dp spacing system for padding/gaps/section spacing | Random spacing increments with no rhythm |
| **Readable text measure** | Keep long-form text readable on large devices (avoid edge-to-edge paragraphs on tablets) | Full-width long text that hurts readability |
| **Section spacing hierarchy** | Define clear vertical rhythm tiers (e.g., 16/24/32/48) by hierarchy | Similar UI levels with inconsistent spacing |
| **Adaptive gutters by breakpoint** | Increase horizontal insets on larger widths and in landscape | Same narrow gutter on all device sizes/orientations |
| **Scroll and fixed element coexistence** | Add bottom/top content insets so lists are not hidden behind fixed bars | Scroll content obscured by sticky headers/footers |

---

### Pre-Delivery Checklist (canonical — the only one)

Before delivering app UI code, verify every item below. Start with the process steps, then the per-area checkboxes.

#### Process
- [ ] Consulted only the rules relevant to this interface (e.g. the modal/keyboard-focus rules in the rule set above)
- [ ] Re-reviewed rule sets 1–3 (CRITICAL + HIGH) as a final pass
- [ ] Tested on 375px (small phone) and in landscape orientation
- [ ] Verified behavior with **reduced-motion** enabled and **Dynamic Type**/largest system text size
- [ ] Checked dark mode contrast independently (never assume light-mode values carry over)
- [ ] Confirmed all touch targets ≥44pt and no content hidden behind safe areas

#### Visual Quality
- [ ] No emojis used as icons (use SVG instead)
- [ ] All icons come from a consistent icon family and style
- [ ] Official brand assets are used with correct proportions and clear space
- [ ] Pressed-state visuals do not shift layout bounds or cause jitter
- [ ] Semantic theme tokens are used consistently (no ad-hoc per-screen hardcoded colors)

#### Interaction
- [ ] All tappable elements provide clear pressed feedback (ripple/opacity/elevation)
- [ ] Touch targets meet minimum size (>=44x44pt iOS, >=48x48dp Android)
- [ ] Micro-interaction timing uses shared, platform-appropriate tokens and remains responsive in context
- [ ] Disabled states are visually clear and non-interactive
- [ ] Screen reader focus order matches visual order, and interactive labels are descriptive
- [ ] Gesture regions avoid nested/conflicting interactions (tap/drag/back-swipe conflicts)

#### Light/Dark Mode
- [ ] Primary text contrast >=4.5:1 in both light and dark mode
- [ ] Normal primary and secondary text contrast >=4.5:1 in both light and dark mode
- [ ] Dividers/borders and interaction states are distinguishable in both modes
- [ ] Modal/drawer scrim is measured against the real background and preserves foreground legibility
- [ ] Both themes are tested before delivery (not inferred from a single theme)

#### Layout
- [ ] Safe areas are respected for headers, tab bars, and bottom CTA bars
- [ ] Scroll content is not hidden behind fixed/sticky bars
- [ ] Verified on small phone, large phone, and tablet (portrait + landscape)
- [ ] Horizontal insets/gutters adapt correctly by device size and orientation
- [ ] 4/8dp spacing rhythm is maintained across component, section, and page levels
- [ ] Long-form text measure remains readable on larger devices (no edge-to-edge paragraphs)

#### Accessibility
- [ ] Decorative icons beside visible text are hidden from the accessibility tree (`aria-hidden="true"` on web or the native equivalent)
- [ ] Meaningful images/icons without equivalent visible text have a text alternative
- [ ] Icon controls have an accessible name and announce applicable selected/pressed/expanded state
- [ ] Form fields have labels, hints, and clear error messages
- [ ] Color is not the only indicator
- [ ] Reduced motion and dynamic text size are supported without layout breakage
- [ ] Sticky UI and overlays do not obscure keyboard focus
- [ ] Dragging and swipe-only interactions have button/keyboard alternatives
- [ ] Authentication allows password managers and paste, with a non-cognitive alternative
- [ ] Auto-rotating content has pause/stop controls and stops on focus or reduced motion
- [ ] Failed forms retain inline field errors; multi-error forms also focus a linked error summary after submit


---

## Tips for Better Results
- Pick one dominant intent per decision (palette, style, layout, a11y) and resolve it against the matching table or rule category before moving on.
- Verify the chosen row actually fits the user's product and platform before applying it; adapt rather than copy.
- For accessibility work, check one observable outcome at a time (e.g. error summary after failed submit, focus not obscured by sticky UI, dragging alternatives, accessible authentication).
- For text-layout bugs (wrapping headings, compact badges/chips, long tokens), apply the typography/layout rules (`heading-line-balance`, `compact-label-overflow`, `chip-collection-reflow`, `long-token-wrapping`) before reaching for framework hacks.

| Problem | Where to look in Part 1 |
|---|---|
| Can't decide on style/color | Re-check Part 2A/2B with different product keywords; apply rule set 4 and 6 |
| Dark mode contrast issues | Rule set 6: `color-dark-mode`, `color-accessible-pairs` |
| Animations feel unnatural | Rule set 7: `spring-physics`, `easing`, `exit-faster-than-enter` |
| Form UX is poor | Rule set 8: `inline-validation`, `error-clarity`, `focus-management` |
| Navigation feels confusing | Rule set 9: `nav-hierarchy`, `bottom-nav-limit`, `back-behavior` |
| Layout breaks on small screens | Rule set 5: `mobile-first`, `breakpoint-consistency` |
| Performance / jank | Rule set 3: `virtualize-lists`, `main-thread-budget`, `debounce-throttle` |

## Optional: the original search CLI
If (and only if) the original repository is available locally, its CLI can supplement this file. Invoke it by full path (never assume the working directory), try `python3` or `py -3` if `python` is missing, treat results as recommendations rather than instructions, and never persist unverified output:

```bash
python "<repo>/.claude/skills/ui-ux-pro-max/scripts/search.py" "<2-5 terms>" --design-system [-p "Project"]
python "<repo>/.claude/skills/ui-ux-pro-max/scripts/search.py" "<keyword>" --domain <ux|style|color|typography|chart|landing|product|icons|gsap|react|web>
python "<repo>/.claude/skills/ui-ux-pro-max/scripts/search.py" "<keyword>" --stack <stack>
```
Retry once with a narrower query or an explicit domain/stack if results are empty or off-topic; if still empty, fall back to this file and say so. Optional flags: `--variance/--motion/--density <1-10>`, `--persist --output-dir <project-root>` (never `--force` without explicit user authorization).
