# themes/ KNOWLEDGE BASE

## OVERVIEW
3 self-contained CSS files users copy-paste into Cherry Studio's customization panel. No build step, no @import between them.

## STRUCTURE
| File | Lines | Scope |
|------|-------|-------|
| `maple-neon.css` | 267 | Full theme: fonts + neon marquee border + responsive/accessibility |
| `just-flowing-border.css` | 216 | Border animation only, no font config. Contributed by @XiuDayyds |
| `maple-neon-font-minimal.css` | 267 | Byte-identical to `maple-neon.css` (see ANTI-PATTERNS) |

## WHERE TO LOOK
| Task | File | Notes |
|------|------|-------|
| Modify the theme | `maple-neon.css` | Canonical file; CI syncs it into all READMEs on push |
| Only border animation | `just-flowing-border.css` | Independent subset — strips font config (lines 4-53 of full theme) |
| Z-index stacking | `maple-neon.css` | `::before` = -1 (behind), `input/textarea` = 2 (foreground), `.ai-status-dot` = 3 (top) |
| Browser compat patches | `maple-neon.css` | `@supports` blocks at end: Firefox vs Safari mask rendering |

## CONVENTIONS
- Section dividers use Chinese inline comments: `/* === 字体配置 === */`. English only for browser compat notes.
- Shared across all 3 files: identical `@keyframes` (`ai-running-gradient`, `ai-thinking-pulse`, `ai-data-flow`).
- `#inputbar` is the sole anchor selector. Neon border rendered via `::before` with double-mask technique (`mask` + `-webkit-mask`, `mask-composite: exclude`) for transparent-cutout illusion.
- Activation triggers: `#inputbar:focus-within::before`, `#inputbar.ai-active::before`, `#inputbar[data-ai-status="running"]::before`.
- Gradient palette: `#ff6a01 → #f8c91c → #8a2be2 → #00d4ff → #f8c91c → #ff6a01`. Repeated in `background` + `@keyframes`.
- CSS variables like `var(--user-font-family)` and `var(--user-code-font-family)` are Cherry Studio's own — set by the app, NOT by these themes.
- Responsive: `@media (max-width: 768px)`. Accessibility: `prefers-reduced-motion` + `prefers-contrast: high`.

## ANTI-PATTERNS (THEMES)
- **Stale duplicate**: `maple-neon-font-minimal.css` is byte-for-byte identical to `maple-neon.css` despite naming itself "font-minimal". Either it drifted out of sync or was never trimmed. Delete or split properly.
- **Misleading banner**: All 3 files open with `/* Maple Neon Minimal Theme... */`, including `just-flowing-border.css` which is a different feature subset, not "Minimal". Fix banner to match file purpose.
- **Hardcoded colors everywhere**: All 4 gradient hex values appear in every `::before` + `@keyframes` across all 3 files. If changing palette, hunt down every instance manually — no CSS custom properties defined.
- **`!important` sprawl (5 instances)**: font-family override (1), `@media reduced-motion` (1), `.ai-mode-active` (1), `.ai-mode-disabled` (2). Last 3 are intentional overrides; first 2 may be fixable by restructuring cascade.
- **Duplicated `#inputbar::before` block inside @media**: The `@media (max-width: 768px)` block repeats the `#inputbar::before` selector twice (lines 213-221) — combine into one rule.
