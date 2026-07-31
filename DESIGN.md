# DESIGN.md — Candice site design system

The detector checks new work against these tokens. Additions should be deliberate.

## Typography
- Display / headings: **Young Serif** (h1, h2, brand)
- Body / UI: **Hanken Grotesk** (h3, paragraphs, labels, buttons)
- No monospace. No other families.

### Type scale (few steps, ≥1.25 ratio)
| Token | Size |
|-------|------|
| display | 4rem (mobile 3rem / 2.5rem) |
| h2 | 2.375rem (mobile 2rem / 1.875rem) |
| h3 / lead | 1.375rem (22px) |
| body | 1.0625rem (17px) |
| small | 0.8125rem (13px) |

Functional text never below 13px. Body measure capped 65–72ch.

## Color (warm dark — OKLCH-authored, hex fallbacks documented)
| Token | Value | Use |
|-------|-------|-----|
| --bg | #1b1917 | page |
| --surface | #232019 | cards |
| --raised | #2b261e | nested / inputs |
| --ink | #f4efe6 | headings |
| --body | #d6ccbb | body text |
| --muted | #b3a892 | labels, meta (AA ≥ 4.5:1 on --bg) |
| --accent | #c99a5b | brass — the one accent |
| --accent-strong | #d8ab68 | hover / emphasis |
| --blush | #e0a59c | tiny secondary tint only |
| --line | rgba(244,239,230,.12) | hairline borders |

Rules: no pure #000/#fff, neutrals tinted warm, 60/30/10 weight, accent stays rare.
No glows (no colored box-shadow), no gradient text, no purple/cyan.

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
