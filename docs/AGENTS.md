# DOCS KNOWLEDGE BASE

## OVERVIEW
Multilingual README translations (zh, fr, ja) — all mirror `/README.md` in structure. Each embeds an auto-synced CSS code block from `themes/maple-neon.css`.

## STRUCTURE
```
docs/
├── README.zh.md    # 中文 — canonical translation template
├── README.fr.md    # Français — mirrors zh structure
├── README.ja.md    # 日本語 — mirrors zh structure
└── AGENTS.md       # This file
```

## WHERE TO LOOK
| Task | Location | Notes |
|------|----------|-------|
| Add a new language | `docs/` | Copy `README.zh.md`, translate content, add lang link to all 4 READMEs |
| Fix a translation | `docs/README.{lang}.md` | Only translate prose — never touch CSS block or image paths |
| Update language links | All 4 READMEs | Each has a lang bar linking to every other translation + root English |
| Understand translation workflow | Root `README.md` first | All 3 translations follow its section sequence exactly |

## CONVENTIONS
- **`README.zh.md` is the template** — new translations clone its structure, not root README
- **Language bar at top** — every translation lists all available languages with relative GitHub links
- **CSS block is readonly** — CI (`sync-theme-to-readme.yml`) overwrites the ` ```css ` block in ALL 4 READMEs on push to `themes/maple-neon.css`
- **Image paths are relative** — all screenshots reference `../examples/main-page-*.png` (not `/examples/`)
- **Font download links** — every translation repeats the same Maple Mono + DreamHan download URLs; keep them identical across languages unless locale-specific mirrors exist
- **Section titles translate, structure stays** — heading order mirrors root README exactly. Only prose changes.
- **Detail/Summary tags** — each translation wraps the CSS block in `<details><summary>...</summary>` with a localized prompt (e.g., "或者，如果你不想修改…" for zh, "Ou, si vous ne voulez pas modifier…" for fr)
