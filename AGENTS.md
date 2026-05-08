# PROJECT KNOWLEDGE BASE

**Generated:** 2026-05-08
**Commit:** 650d127
**Branch:** master

## OVERVIEW
Maple Neon — a pure CSS theme for Cherry Studio (desktop LLM client). Mixes Maple Mono font family with neon-styled input bar marquee animations. Zero build system, no JS/TS — users copy-paste CSS into Cherry Studio's customization panel.

## STRUCTURE
```
./
├── themes/       # Deliverable theme CSS (3 variants)
├── templates/    # Cherry Studio upstream CSS reference (gitignored, synced via script)
├── docs/         # Multilingual README translations (zh, fr, ja)
├── examples/     # Screenshots (light/dark mode)
├── .github/      # CI: auto-syncs CSS into READMEs on push
├── .idea/        # JetBrains IDE config (SCSS watcher disabled)
└── .vscode/      # VS Code: AI assistant language set to zh-CN
```

## WHERE TO LOOK
| Task | Location | Notes |
|------|----------|-------|
| Modify the theme | `themes/maple-neon.css` | Canonical file; CI syncs to all READMEs |
| Understand Cherry Studio's CSS architecture | `templates/index.css` | @import chain into 11 sub-files |
| Update Cherry Studio reference CSS | `templates/update-template.bat` | Downloads from upstream GitHub |
| Add a translation | `docs/README.{lang}.md` | Mirror structure of `README.zh.md` |
| Preview screenshots | `examples/` | main-page-light.png / main-page-dark.png |
| CI pipeline | `.github/workflows/sync-theme-to-readme.yml` | Triggers on push to `themes/maple-neon.css` |

## CONVENTIONS
- **No build step** — theme ships as raw CSS. No `package.json`, no bundler, no npm.
- **`templates/` is gitignored but tracked** — historical artifact; the directory exists in repo but `.gitignore` lists `/templates`. Do not rely on it being ignored.
- **Commit style**: `docs: ... [skip ci]` for CI auto-commits.
- **Comments**: Chinese inline, English for structural headers. Section dividers use `/* === SECTION === */`.
- **Font stack**: Maple Mono NF CN (code) + Microsoft YaHei (UI) + DreamHan Sans/Serif (Chinese). All optional — have fallbacks.

## ANTI-PATTERNS (THIS PROJECT)
- **License mismatch**: `LICENSE` = AGPL v3, but `README.md` still claims MIT. Do NOT trust the README badge.
- **`!important` sprawl**: 30+ `!important` across theme and template files. Avoid adding more — restructure cascade instead.
- **Hardcoded gradient colors**: `#ff6a01`, `#f8c91c`, `#8a2be2`, `#00d4ff` are inline in all 3 theme files. Extract to CSS custom properties if modifying.
- **Unsysteatic z-index**: Uses raw values -1, 1, 2, 3 without a documented scale. Document if adding new layers.
- **Dead code in templates**: See `templates/AGENTS.md` for specific locations and cleanup guidance.

## UNIQUE STYLES
- **Theme variant drift**: `maple-neon-font-minimal.css` is currently byte-for-byte identical to `maple-neon.css` (see `themes/AGENTS.md`). `just-flowing-border.css` is the true animation-only subset. If splitting variants, sync shared sections across all three.
- **CSS embedded in READMEs**: All 4 READMEs contain an inline ` ```css ` block auto-synced by CI. NEVER manually edit the CSS block in READMEs — it will be overwritten.
- **Bilingual comments**: CSS uses Chinese comments (`/* 字体配置 */`) for section headers. New code should follow this style.

## COMMANDS
```bash
# Update Cherry Studio reference templates
cd templates && .\update-template.bat

# Check what happens when theme is pushed (CI dry-run)
# CI triggers on: push to main/master, file changed: themes/maple-neon.css
```

## NOTES
- **Cherry Studio compatibility**: Theme targets Cherry Studio v1.2.4+. Uses attribute selectors (`[os="windows"]`, `[theme-mode='light']`) which may break if Cherry Studio changes DOM conventions.
- **FIXMEs in templates/scrollbar.css**: Two known workarounds — html-to-image pseudo-element bug + antd select scrollbar override. These are upstream issues, not theme bugs.
- **No tests**: Pure CSS project. Visual testing done manually by pasting into Cherry Studio.
- **Font downloads required**: Maple Mono NF CN and DreamHan fonts are recommended but optional. See `README.md` for download links.
