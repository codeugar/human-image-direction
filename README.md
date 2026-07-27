# human-image-direction

Grok / Claude-style **skill** for directing photoreal images of **clearly adult** women with high attraction — via **造型 (styling craft)** and professional photography language, **not** by stacking “sexy / hot” keywords.

Default visual register: **fashion editorial**, **fashion advertising**, **sports / activewear**, and **commercial beauty / lifestyle** photography.

## Install

### Grok (user skill)

```text
~/.grok/skills/human-image-direction/SKILL.md
~/.grok/skills/human-image-direction/references/prompt-templates.md
```

Copy this repo into:

```bash
# Windows
%USERPROFILE%\.grok\skills\human-image-direction\

# macOS / Linux
~/.grok/skills/human-image-direction/
```

Or symlink / clone there. Slash invoke: `/human-image-direction`.

### Claude Code / other skill loaders

Place the folder under your tool’s skills path so `SKILL.md` frontmatter is discovered.

## What’s inside

| File | Role |
|------|------|
| `SKILL.md` | Main agent instructions |
| `references/prompt-templates.md` | Fill-in templates and seed expansions |

## Core ideas

### North star: 用造型表达吸引力

Attraction is **designed**, not labeled. Build pull from silhouette, fit, fabric, cut, pose, gaze, lens, and three-point light.

### Stable foundations (concrete description)

0. **Style** — fashion: **editorial XOR fashion advertising** (or beauty / sport / product)  
1. **服装材质与剪裁** — material + cut + opacity + coverage  
2. **身体朝向和重心** — orientation + weight  
3. **表情与视线** — gaze target + lids + mouth + chin  
4. **镜头焦段** — mm + DOF + crop  
5. **主光 / 补光 / 轮廓光** — key + fill + rim (or explicit none)

### Product / AI marketing gate (§−1)

When the brief is a **product, app, AI model, or marketing keyword** (e.g. AI image editor):

1. **Do not generate immediately**
2. **Confirm with the user** which **model core capability** and/or **use case** to showcase
3. Map that brief onto on-screen UI / props
4. Then apply Style #0 + five foundations and generate

### Canonical premium editorial seed

Expand every time (never leave as short tags only):

```text
relaxed three-quarter pose, confident expression,
gaze slightly past camera, opaque technical satin outfit,
soft window light with clean bounce flash,
premium editorial fashion photography
```

## Hard gates

- Concrete adult age **24–32** (e.g. `26-year-old`)
- Opaque clothing; no accidental sheer / underage framing
- No real named people without consent-safe reference workflow
- Identity anchors across a series (not only “same woman”)

## License

MIT — see [LICENSE](LICENSE).

## Author notes

Intended for agents (Grok Build, Claude Code, etc.) as a reusable prompt-direction package. Review and adjust for your product safety and brand guidelines before production use.
