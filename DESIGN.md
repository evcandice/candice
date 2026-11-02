# DESIGN.md — Candice site design system

The detector checks new work against these tokens. Additions should be deliberate.

## Typography
- **Comfortaa** is the site's official typeface: one family for headings, body and UI, weights 300–700 (variable), SIL Open Font License, loaded from Google Fonts. The `--serif` and `--sans` tokens both point at it.
- Weights: h1, h2, h3 and the brand 700; body 400; buttons and nav 600; small functional text (13px) 500, because Comfortaa's 400 strokes are thin at that size. h1/h2 letter-spacing -.01em.
- No monospace. No other families. This is the website only; the app keeps its own type.

### Type scale (few steps, ≥1.25 ratio; tuned for Comfortaa's wider, rounder letters)
| Token | Size |
|-------|------|
| display | 3.5rem (≤820px 2.75rem / ≤480px 2.25rem) |
| h2 | 2.125rem (≤820px 1.875rem / ≤480px 1.625rem) |
| h3 / lead | 1.25rem (20px) |
| body | 1.0625rem (17px), line-height 1.7 |
| small | 0.8125rem (13px), weight 500 |

Functional text never below 13px. Body measure capped 65–72ch.

## Color (warm paper, matched to the octopus illustration)
| Token | Value | Use |
|-------|-------|-----|
| --bg | #FBF7EC | page — the paper the octopus sits on |
| --surface | #FFFCF4 | cards, a shade above the page |
| --raised | #FFFEFA | nested / inputs |
| --ink | #231A12 | headings — the octopus's outline ink, warm, never pure black |
| --body | #4D4234 | body text (9.15:1 on --bg) |
| --muted | #766A58 | labels, meta (4.94:1 on --bg) |
| --accent | #ED6E19 | orange — the shadow side of the octopus, the one accent |
| --accent-strong | #A84503 | hover / emphasis; AA on the page (5.58:1) and on the token tint (5.02:1) |
| --blush | #E89A72 | tiny secondary tint only |
| --line | rgba(35,26,18,.13) | hairline borders |

The primary button carries a dark label at rest and a near-white one on hover, because
--accent-strong is too dark for the dark label.

Rules: no pure #000/#fff, neutrals tinted warm, 60/30/10 weight, accent stays rare.
No glows (no colored box-shadow), no gradient text, no purple/cyan.
The iPhone frame keeps its own dark greys — it is a photograph of a device, not chrome.

## Shape / radius
| Token | Value |
|-------|-------|
| --r-sm | 8px |
| --r-md | 12px |
| --r-lg | 16px |
| pill | 999px (tags, buttons only) |
Cards top out at 16px. Exception: the iPhone device frame (48px) — it's a real device.

## Space (4pt scale)
4, 8, 12, 16, 24, 32, 48, 64, 96 — via --space-* tokens. Use `gap`, vary for rhythm.

## Surfaces
Commit to ONE: a defined hairline edge OR a soft elevation — never border + wide shadow
together. Surface cards use the edge and lie flat. Only the floating device uses shadow.

## Motion
Entrance reveals only (opacity + transform, ease-out). No pulsing status dots, no bounce,
never animate width/height/padding. Respect prefers-reduced-motion.
