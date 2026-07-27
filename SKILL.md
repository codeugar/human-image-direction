---
name: human-image-direction
description: >
  Prompt-direction for photoreal attractive adult women — north star 用造型
  表达吸引力 (styling craft, not sexy keywords): BEFORE generating for
  product/AI/app ads, confirm with the user which model core capability or
  use case to showcase; identity anchors, adult age, material and cut, pose
  and weight, expression and gaze, lens, key/fill/rim, editorial vs fashion-ad
  style, product co-frame, cross-image consistency. Prefer lookbook, editorial,
  campaign, activewear, beauty ads. Use for lifestyle, beauty, fashion,
  sportswear, social, ad, AI product, alluring / 擦边 woman shots, or
  /human-image-direction.
metadata:
  short-description: "Attractive adult woman via styling craft"
---

# Human Image Direction

## North star: 用造型表达吸引力

Attraction is **designed**, not labeled. Never stack "sexy / hot /
beautiful / 勾人". Build pull from **造型 + 摄影工艺**:

| 造型杠杆 | What to specify | Attraction effect |
|----------|-----------------|-------------------|
| Silhouette / 廓形 | column, cropped+high-waist long-leg, tailored hourglass | body line readable without undress language |
| Fit / 合身 | tailored, body-skimming, structured, compression | cloth meets body with intent |
| Material / 材质 | technical satin, matte silk, ribbed knit + opacity | light and touch read as premium |
| Cut / 剪裁 | neckline geometry, rise, hem, coverage lock | collarbone/leg line via pattern, not “露” |
| Pose + 重心 | relaxed ¾, weight on back leg, free knee soft | natural S-curve, not passport stiff |
| 表情与视线 | confident lids/mouth; lens, past-camera, or off-frame | presence without bedroom tokens |
| 焦段 + 裁切 | 50–85mm, mid-thigh / chest-up | compression and hero frame |
| 主光/补光/轮廓 | window key + bounce fill, softbox, rim | shape face, fabric, hair edge |
| Genre | premium editorial / fashion ad / beauty / sport | professional register sells aspiration |

**Rewrite rule:** if the user only says 好看/性感/擦边/attractive, convert
into the table above before calling `image_gen` / `image_edit`.

**Compact seed pattern (expand every time):**  
`[editorial XOR fashion ad | other genre] + [pose + 重心] + [expression + gaze] + [opaque material + cut] + [mm + DOF + crop] + [key + fill + rim]`

Canonical example seed (full — always expand; see Stable foundation):

```
relaxed three-quarter pose, confident expression,
gaze slightly past camera, opaque technical satin outfit,
soft window light with clean bounce flash,
premium editorial fashion photography
```

= Style #0 editorial + 朝向重心 + 表情视线 + 材质(须补剪裁) + 窗光主光/跳闪补光
(+ 默认 85mm mid-thigh if mm omitted).

**Default visual register:** 现有资料与默认语域偏向 **专业时尚、运动和
商业摄影**（fashion / sports / commercial），**不是**直接堆叠 sexy。

Load this skill whenever you are about to call `image_gen` or
`image_edit` for a woman subject (solo or with product). Follow every
section below even when the user only says "画个好看的女生".

## −1. Capability / use-case confirmation (input gate — before prompting)

When the request involves a **product, app, AI model, SaaS, or marketing
keyword** (e.g. AI image editor, AI photo app, beauty tool, campaign for
X), **do not immediately generate**. First **confirm with the user** what
to showcase.

### When this gate is mandatory

Trigger if **any** of:

- product / app / AI / model / tool / editor / SaaS is named or implied  
- user says “围绕关键词出图” / “产品图” / “带货” / “落地页” / “hero”  
- woman + screen / phone / UI / device co-frame  
- campaign set where the **value prop** is not pure fashion portrait  

Skip only for pure fashion / beauty portrait with **no product narrative**.

### What to confirm (ask clearly; wait for answer)

Ask the user to pick **one primary focus** (or rank top 2–3 for a series):

**A — 模型核心能力 (model core capabilities)**  
What capability should the frame prove?

Examples (adapt to product): text-to-image · image-to-image · inpaint /
generative fill · upscale / enhance · background remove · style transfer ·
face restore · batch edit · real-time preview · one-tap AI enhance ·
before/after quality · multi-layer control · prompt bar / controlnet-style
controls · etc.

**B — Use case (场景用例)**  
Who is doing what, for what outcome?

Examples: social creator retouching a selfie · e-comm seller cleaning
product shots · designer fixing a poster · travel photo sky replace ·
portrait studio skin polish · marketer making ad variants · etc.

Also confirm if needed:

- **Hero hierarchy:** face vs UI/screen vs device  
- **Channel:** 9:16 story / 3:4 feed / 16:9 landing hero  
- **Style #0:** editorial vs fashion advertising vs pure product UI  

### How to ask (template)

Use a short confirmation, not a vague “what do you want?”:

> 出图前确认一下要主打什么（可多选排序）：  
> **1) 核心能力** — 例如：一键增强 / 局部重绘 inpaint / 去背景 / 前后对比 / 文生图…  
> **2) Use case** — 例如：博主修自拍 / 电商修产品图 / 设计师改海报 / 旅行照换天…  
> 另外：画面以 **人像 / 屏幕 UI / 设备** 谁为主？竖版还是横版？

Prefer `ask_user_question` when available (multi-select for capabilities /
use cases + single-select for hero hierarchy). Otherwise ask in plain text
and **wait**.

### After the user answers

1. Lock a one-line **Capability brief** (e.g. `primary: AI enhance before/after on mobile; secondary: creator lifestyle`).  
2. Map brief → visible props in the prompt (UI state, on-screen action,
   grip, hero hierarchy) — see §7.  
3. Only then run Style #0 + five foundations + `image_gen` / `image_edit`.  

If the user already named a clear capability **and** use case in the same
message, restate the brief in one line and proceed (no re-ask). If only a
keyword like “AI image editor” with no capability/use case → **always ask**.

## Stable foundation: 更稳定的方式是具体描述

比 vibe 词更稳的是 **逐项写具体细节**。顺序：  
**能力/用例确认（§−1）→ 风格 #0 → 五条基座**。

### 0) 编辑摄影或时尚广告风格（§3.2 — 时装必选）

时装类 **必须二选一**（不要混成 “fashion sexy shoot”）：

| | **编辑摄影** | **时尚广告** |
|--|--------------|--------------|
| Tag | fashion magazine editorial / premium editorial fashion photography | fashion advertising campaign still / luxury brand campaign |
| 目标 | 情绪、叙事、艺术指导 | 品牌、渴望、可连镜头的英雄主体 |
| 视线 | slightly past camera / off-frame | campaign stare / soft lens lock |
| 光 | 方向主光 · 少补光 · 更强 rim | softbox · 可控补光 · subtle rim |
| 焦段 | 35–50mm 环境 或 85mm + 负空间 | 50–85mm mid-thigh hero |

用户只说「好看时装」→ 默认 **时尚广告**；说杂志/大片/情绪 → **编辑**。

### 1) 服装材质与剪裁（§3.4 — mandatory）

Always name:

- **材质 + 手感**（matte silk drape / technical satin / ribbed knit…）
- **光泽**（matte 或 controlled sheen）
- **剪裁几何**（领线落点、腰高、下摆长度）
- **合身**（tailored / body-skimming / structured / compression）
- **不透明 + 覆盖**（fully opaque；遮住胸/胯）

禁止只写：nice outfit / fashion clothes / sexy dress。

### 2) 身体朝向和重心（§3.5 — mandatory）

Always name:

- **身体相对镜头的朝向**（正对 / 15–30° / ¾ 约45° / 回眸）
- **头与躯干是否分离**（head to lens / torso away）
- **重心在哪条腿**（weight on back/left/right/front leg）
- **自由腿状态**（soft knee / extended）与髋肩结果

禁止只写：standing nicely / good pose / sexy pose。

### 3) 表情与视线（§3.3 — mandatory）

Always name (face visible):

- **视线目标**（to lens / slightly past camera / off-frame / over-shoulder /
  along path / to product）
- **眼睑能量**（half-lid soft / bright open / alert athletic / narrow editorial）
- **嘴型**（closed calm / closed-lip hint smile / soft parted / neutral editorial）
- **下巴微角**（level / slightly lowered / slight ¾ turn）
- 脸英雄时加 **catchlights in both eyes**

禁止只写：beautiful face / sexy eyes / 迷人眼神 / confident look（无几何细节）。

### 4) 镜头焦段（§3.6 — mandatory）

Always name **focal-length feel** (full-frame equivalent) + **DOF intent** +
**crop** when framing is set:

| mm | 写什么 | 何时用 |
|----|--------|--------|
| **35mm** | 35mm environmental; wider context | 运动全身、地点编辑、环境故事 |
| **50mm** | 50mm natural perspective; moderate DOF | lookbook ¾、均衡广告 |
| **85mm** | 85mm portrait compression; shallow DOF | 美妆英雄、时装半身、浅景深 |
| **105mm** | 105mm beauty tele; tight face planes | 紧面部、皮肤细节 |

禁止只写：cinematic / bokeh / DSLR / professional photo（无 mm）。

### 5) 主光、补光和轮廓光（§3.7 — mandatory）

Always name a **three-point (or reduced) setup** by **role + direction +
quality**. Professional fashion / sports / commercial lighting — not
"sexy lighting" or empty "cinematic light".

| 光位 | 必写 | 示例短语 |
|------|------|----------|
| **主光 Key** | 方向 + 软/硬 + 光源 | soft window key camera-left 45°; large softbox key; sun as side key |
| **补光 Fill** | 强度 + 侧别（或明确 minimal） | gentle fill camera-right; clean bounce flash fill; weak sky fill; minimal fill |
| **轮廓光 Rim** | 有/无 + 落点 | subtle hair rim from behind; cool edge on far shoulder; none |

禁止只写：good lighting / studio light / cinematic light / moody light。

#### Full premium editorial seed (always expand — never leave bare)

Canonical user seed (two lines often pasted separately; treat as **one**):

```
relaxed three-quarter pose, confident expression,
gaze slightly past camera, opaque technical satin outfit,
soft window light with clean bounce flash,
premium editorial fashion photography
```

| Seed phrase | Foundation | Expand to |
|-------------|------------|-----------|
| **relaxed three-quarter pose** | 朝向+重心 | torso ~45°; weight on back leg; front knee soft; shoulders relaxed; near shoulder slightly forward; optional free-side hand on hip |
| **confident expression** | 表情 | open unhurried lids (not glare); closed calm mouth or closed-lip hint smile; chin slightly turned |
| **gaze slightly past camera** | 视线 | primary gaze = slightly past the camera (near-lens, not locked); both eyes same direction; catchlights if face hero |
| **opaque technical satin outfit** | 材质+剪裁 | opaque technical satin, controlled sheen, not sheer; **must add** neckline/cut, fit, what it covers (chest etc.) |
| **soft window light with clean bounce flash** | 主光+补光 | key = soft window (name side, e.g. camera-left); fill = clean bounce flash (opposite, soft not on-camera harsh); rim optional faint hair |
| **premium editorial fashion photography** | Style #0 | genre = premium fashion **editorial** (not fashion advertising, not sexy snapshot); defaults: past-camera gaze, window+bounce, 50–85mm, negative space OK |

**Default lens when seed omits mm:** 85mm shallow mid-thigh **or** 50mm moderate mid-thigh with negative space.

**Expanded full prompt (paste and fill identity + cut):**

> Exactly one {age}-year-old adult woman [identity locks]. Premium
> editorial fashion photography. Relaxed three-quarter pose: torso about
> 45° to camera, weight on her back leg, front knee soft, shoulders
> relaxed. Confident expression: open unhurried lids with catchlights,
> closed calm mouth, chin slightly turned; gaze slightly past the camera
> (near-lens, not locked). Outfit: opaque technical satin [ADD cut:
> neckline, fit, coverage — e.g. column dress, crew neckline, fully
> covering the chest to mid-calf], controlled sheen only. Soft window key
> from camera-left; clean bounce flash fill from camera-right; optional
> faint hair rim. Shot on 85mm, shallow depth of field, mid-thigh crop.
> Natural pores, technical satin surface detail. Photoreal, two hands,
> no doll skin, fully opaque clothing.

#### Pose + expression mini-seed only

If user writes only:

```
relaxed three-quarter pose, confident expression,
```

still expand pose + face; **add a gaze** if missing:

- with this full editorial seed → use **gaze slightly past camera**  
- fashion advertising → calm confident gaze to the lens  
- beauty → soft direct to lens  

**Advertising pose+face block**  
> Relaxed three-quarter pose: torso about 45° to camera, weight on her
> back leg, front knee soft, shoulders relaxed, free-side hand on hip.
> Confident expression: open unhurried lids with catchlights, closed calm
> mouth, chin slightly turned; calm confident gaze straight into the lens.

**Editorial pose+face block (matches full seed)**  
> Relaxed three-quarter pose: torso about 45° to camera, weight on her
> back leg, front knee soft, shoulders relaxed. Confident expression:
> open unhurried lids with catchlights, closed calm mouth, chin slightly
> turned; gaze slightly past the camera (near-lens, not locked).

**基座句模（风格 + 五条 — 优先粘贴）：**

> Fashion advertising campaign still. Relaxed three-quarter pose: torso
> about 45° to camera, weight on her back leg, front knee soft; free-side
> hand on hip. Confident expression: open unhurried lids, closed calm
> mouth; calm confident gaze to the lens. Outfit: opaque [material],
> [neckline/cut], [fit], fully covering the chest; [bottom + rise/hem].
> Shot on 85mm, shallow depth of field, mid-thigh crop. Large softbox key
> camera-left; controlled fill camera-right; subtle hair rim. Not a sexy
> snapshot.

> Fashion magazine editorial. Relaxed three-quarter pose: torso about 45°
> to camera, weight on back leg, front knee soft, shoulders relaxed.
> Confident expression: open unhurried lids, closed calm mouth, chin
> slightly turned; gaze slightly past the camera. Outfit: opaque
> [material], [cut + coverage]. Shot on 50mm, moderate DOF, mid-thigh with
> negative space. Soft window key camera-left; clean bounce flash fill;
> optional faint hair rim.

## 0. Hard gates (non-negotiable)

1. **Clearly adult.** Every prompt must state a concrete adult age in the
   **24–32** range (pick one number per identity, e.g. `26-year-old`).
   Never use teen, schoolgirl, young girl, 少女, 萝莉, or age-ambiguous
   "youthful" framing.
2. **Clothed.** Skin may show strategically (collarbone, décolletage,
   midriff, thighs) but **opaque fabric covers breasts and groin**. No
   sheer-to-nude accidents, no full nudity unless the user explicitly
   requests and policy allows.
3. **No real named people** without a consent-safe reference via
   `image_edit` (see imagine skill). Invented models only for pure
   `image_gen`.
4. **One coherent scene** per prompt. Prefer positive description over
   long negative lists (but keep the short anti-artifact block in §6).
5. **Capability / use-case confirmed** when product/AI/marketing keyword
   is in play (§−1). Do not invent which feature to demo without user
   input (unless they already stated it clearly).

## 1. Identity card (write before prompting)

Before the first image of a person, lock an **identity card**. Reuse it
verbatim across the set. Do **not** rely on "same woman".

Minimum locks (pick concrete values):

| Slot | Example values |
|------|----------------|
| Age | 26-year-old adult woman |
| Face shape | soft oval with slightly wider cheekbones |
| Eyes | warm hazel, slightly almond, faint natural crease |
| Brows | medium-thick, gently arched, not over-plucked |
| Nose | small straight bridge, soft tip |
| Mouth | full lower lip, natural peach tone |
| Hair | shoulder-length dark brown with soft waves, center-part, fine baby hairs at hairline |
| Signature mark | tiny freckle mole under left eye (her left) |
| Asymmetry | right eyebrow a hair higher; smile lifts left cheek more |
| Body note | long legs, natural waist-to-hip curve (keep proportional, not cartoon) |
| Skin | light-medium warm undertone, visible natural pores |

Optional secondary locks (only if needed for series): ear shape, jaw
softness, collarbone prominence, habitual jewelry (thin gold hoop).

**Prompt rule:** put 4–6 highest-signal locks in every image of her.
Signature mark + hair + eye color + face shape is usually enough.

## 2. Realism texture (always include 2–3 cues)

Pick from this menu; do not dump the whole list:

- natural skin pores on cheeks and nose
- fine facial micro-texture, subtle under-eye texture (not heavy bags)
- individual hair strands / flyaways at the crown and temples
- soft catchlights in both eyes, slight moisture on lips
- fabric weave / knit / leather grain matching the garment

Avoid: airbrushed porcelain, plastic sheen, wax-museum stiffness,
perfect bilateral symmetry, doll-like oversized eyes, uncanny smooth
forehead.

## 3. Attraction through 造型 (styling craft)

Operational detail for the north star **用造型表达吸引力**. Express pull
through **concrete styling + photography**, not adjective piles. If a
prompt only says "好看 / 性感 / sexy / 吸引人", **rewrite it** into the
north-star lever table and §3.1 before calling the image tool.

**Minimum 造型 sentence (include something like this every time):**

> Relaxed three-quarter stance, weight on back leg; opaque [material]
> [cut + coverage]; [gaze]; [window/softbox key] with [fill]; [editorial
> or campaign] fashion photography.

### 3.1 Stable description checklist (fill these, not vibes)

More stable than vibe words: spell out each slot in plain specifics.

| Slot | What to name | Concrete examples (use, don't stack all) |
|------|----------------|------------------------------------------|
| **Genre lane** | one pro register | fashion editorial · fashion advertising / campaign · lookbook · beauty campaign · activewear · lifestyle e-comm |
| **Silhouette** | body line the clothes create | column sheath · cropped + high-waist long-leg · hourglass belt cinch · athletic fitted set |
| **Fit** | how fabric sits | tailored close at waist · body-skimming not tight · structured shoulder · compression legging |
| **Cut / neckline** | geometry + coverage | deep V to mid-sternum · square neck · off-shoulder cuff below collarbones · bandeau straight across upper chest · scoop sports bra |
| **Length / crop** | hem and skin map | crop above navel · high-thigh shorts · midi slit to knee · full-length tailored pant |
| **Material + hand** | fiber + surface | matte silk drape · ribbed knit stretch · brushed cotton · technical jersey · soft leather grain · cashmere loft |
| **Opacity** | always lock | fully opaque; no sheer; no wet-look transparency |
| **Color / contrast** | palette role | monochrome black · dusty rose vs warm skin · high-contrast cream on tan · brand-color block |
| **Grooming** | commercial beauty level | clean dewy skin · soft neutral makeup · defined brow · natural lip moisture · hair: soft waves / sleek low pony / sport pulled-back |
| **Jewelry / props** | 0–2 pieces max | thin gold hoops · delicate chain · watch · water bottle · product bottle |
| **Pose language** | fashion/sport, not mugshot | contrapposto weight shift · walking stride freeze · over-shoulder lookbook turn · hands-on-hips power stance · post-run cool-down lean |
| **表情与视线** | gaze target + lid/mouth/chin | soft direct to lens · campaign stare · over-shoulder glance · eyes to product then lens · half-lid calm · closed-lip hint smile |
| **镜头焦段** | mm + DOF/compression feel | 85mm portrait shallow DOF · 50mm natural · 35mm environmental · 105mm beauty |
| **主光/补光/轮廓光** | three-point roles | soft key camera-left 45° · gentle fill opposite · hair rim from behind |
| **Crop** | frame intent | chest-up beauty · mid-thigh lookbook · full-body hero |

**Stability rule (ordered):**

−1. **Capability / use case (when product/AI):** confirm with user (§−1)
0. **Style (never skip):** 时装 → **编辑摄影 XOR 时尚广告**（带 tag 短语）；
   非时装 → beauty / activewear / lookbook / product 之一  
1. **Five foundations (never skip):** 材质剪裁 + 朝向重心 + 表情视线 +
   焦段 + 主光/补光/轮廓光（表情/光/焦段对齐所选风格默认）  
2. **Then:** set / props polish · map capability brief onto UI/props  

Professional fashion / sports / commercial — **not sexy stacking**.

If short: **genre tag + five foundations only**. Never drop style choice,
material/cut, orientation/weight, gaze (if face), mm/DOF, or named key
(+ fill/rim stated, even if `minimal fill` / `no rim`).

### 3.2 编辑摄影或时尚广告风格 (style foundation #0)

**时装类必选。** 编辑摄影与时尚广告 **不是同一套语言**；混写会飘成
「又想艺术又想带货」的模糊图。Prefer these over "sexy shoot".

#### Side-by-side (choose one)

| Slot | **编辑摄影 Editorial** | **时尚广告 Fashion advertising** |
|------|------------------------|----------------------------------|
| Goal | mood, story, art direction; not always selling SKU | sell brand / aspiration; hero readable |
| Tone | cool, intentional shadow, negative space | polished, aspirational, brand-safe |
| Pose | angular, less “friendly”; ¾ relaxed OK | confident lookbook / campaign stance |
| Gaze | slightly past camera, off-frame, cool stare | campaign stare or soft lens lock |
| Mouth | neutral / closed calm; smile rare | neutral or closed-lip hint smile |
| Light | directional key · low fill · stronger rim | soft large key · controlled fill · subtle rim |
| Lens | 35–50mm env **or** 85mm + negative space | 50–85mm mid-thigh / beauty hero |
| Set | architecture, texture, street, sparse art set | seamless, premium interior, clean terrace |
| Color | muted / art-directed | accurate fabric + brand palette |
| Tag phrases | fashion magazine editorial; premium editorial fashion photography; art-directed editorial portrait | fashion advertising campaign still; luxury brand campaign; global campaign billboard crop |

**Decision rule (if user vague on fashion):**

- says 大片 / 杂志 / 情绪 / editorial / 艺术 → **editorial**  
- says 广告 / campaign / 带货 / 种草 / 品牌 / 商业 → **fashion advertising**  
- says only 好看时装 / fashion girl → default **fashion advertising**
  (clearer commercial pull); offer editorial if they want moodier  

#### Editorial photography (编辑摄影 / magazine editorial)

Example package:  
> Fashion magazine editorial; 50mm environmental feel; high-contrast soft
> key from camera-left, minimal fill, cool rim on hair; mid-thigh crop;
> intentional negative space; neutral editorial mouth; gaze slightly past
> camera or off-frame.

#### Fashion advertising / campaign (时尚广告 / brand campaign)

Example package:  
> Fashion advertising campaign still; shot on 85mm with shallow depth of
> field; large softbox key at 45° camera-left, controlled fill camera-right,
> subtle rim separating hair; mid-thigh hero crop; calm campaign gaze to
> lens.

#### Other lanes (still available)

**Lookbook / e-comm fashion** — even lighting, garment clarity first,
often 50–85mm, white or minimal set.

**Sports / activewear campaign** — performance fabric, movement or
cool-down; hard sun or early side key; 35–50mm full body.

**Commercial beauty / lifestyle / product ad** — soft beauty key + fill;
product readable; 85–105mm face or 50mm hand+product.

Mix lanes only when intentional (e.g. editorial × sport: elevated
athleisure story).

### 3.3 表情与视线 (stability foundation #3)

**最稳定、脸可见时必写。** 与材质剪裁、朝向重心并列的第三条基座。
专业时尚 / 运动 / 商业片用 **视线目标 + 眼睑 + 嘴型 + 下巴角** 建立气场，
不是 sexy eyes / 迷人眼神。

Do **not** write only "beautiful face" / "sexy eyes" / "迷人眼神" /
"confident look". Every face-visible prompt names **gaze target**,
**eye aperture**, **mouth shape**, and usually **chin/head micro-angle**.
Keep commercial fashion / beauty energy — not cartoon seduction or duck-face.

#### Gaze target (where the eyes look)

| Target | Prompt phrases | Best for |
|--------|----------------|----------|
| Lens lock | soft direct eye contact with the camera; both eyes to lens | beauty, social hero, connection |
| Campaign stare | calm confident gaze straight into lens; unhurried | fashion lookbook, brand hero |
| Over-shoulder glance | eyes meet lens over the near shoulder; head turned more than torso | lookbook turn, soft allure |
| Off-frame glance | gaze to camera-left / camera-right / slightly above lens | editorial, contemplative, stride |
| Slightly past camera | gaze slightly past the camera (near-lens, not locked); confident editorial | premium editorial fashion, soft intensity |
| Path / activity | eyes along travel direction; optional brief flick to lens | activewear, lifestyle motion |
| Product then face | primary look at product in hand, or eyes to lens while product is held low | commercial co-frame |
| Downcast soft | gaze lowered toward hands / lap, lids relaxed (use sparingly) | quiet beauty, not shy-child |

**Rule:** name a **single primary target**. Split attention only when
product is hero ("eyes to lens; product sharp in lower frame").

#### Eye aperture & brow (energy without vibe words)

Pick **one** lid energy + optional brow note:

| Energy | Concrete phrases |
|--------|------------------|
| Soft half-lid | relaxed half-lidded eyes; calm lower lid; no squint |
| Bright open | open beauty lids; clear iris; awake commercial eyes |
| Focused athletic | alert open eyes; slight focus tension, not glare |
| Narrow editorial | slightly narrowed lids; cool editorial focus |

Brows:

- soft natural brow, not raised surprise
- one brow a hair higher only if it matches identity asymmetry
- no angry knit brow unless intentional editorial

Always add when face is hero: **soft catchlights in both eyes** (matches
key light direction when possible).

#### Mouth & lower face

| Mouth | Prompt phrases |
|-------|----------------|
| Closed calm | lips gently closed, relaxed corners |
| Hint smile | closed-lip hint of a smile; left cheek lifts slightly more (if identity) |
| Soft open | lips softly parted a little; natural, not pout |
| Neutral editorial | neutral mouth, no smile; fashion blank calm |
| Bright commercial | small natural smile showing no teeth or minimal upper teeth only |

**Avoid as default:** exaggerated duck pout, forced bedroom bite, cartoon
smize labels, "seductive lips" without shape words.

#### Chin, head, and face plane (ties to 朝向)

- chin level for athletic / power
- chin slightly lowered 5–15° for beauty flattery and eye dominance
- chin turned 10–20° with face ¾ for bone structure
- head tilt subtle (ear toward shoulder a few degrees) — optional lookbook
- face plane: ¾ face often stronger than dead-frontal mugshot

#### Genre defaults (pick one set if user is vague)

| Lane | Default 表情与视线 package |
|------|---------------------------|
| Beauty campaign | soft direct to lens; half-lid or bright open; closed-lip hint smile; chin slightly down; catchlights |
| Fashion lookbook / campaign | campaign stare or over-shoulder; neutral or hint smile; chin slight turn |
| Premium editorial | gaze slightly past camera **or** off-frame; open unhurried or narrow lids; closed calm / neutral mouth |
| Activewear | alert open eyes to lens or along path; closed calm mouth or light effortless smile; chin level |
| Product ad | eyes to lens (connection) **or** soft look at product; natural closed mouth; chin slight down |

#### Anti-expression block (avoid)

- passport blank stare with no named target
- both eyes looking different directions
- wax-museum frozen grin
- over-wide anime eyes / doll stare
- heavy "sexy" pout + bedroom labels without lid/mouth geometry
- childlike shy peek unless user asks (conflicts with adult commercial read)

#### Expression sentence templates (paste and fill)

**Beauty lens lock**  
> Soft direct eye contact with the camera; relaxed half-lidded eyes with
> catchlights; lips gently closed with a hint of a smile; chin slightly
> lowered.

**Fashion campaign**  
> Calm confident gaze straight into the lens; open but unhurried lids;
> neutral editorial mouth; face in a slight three-quarter turn.

**Over-shoulder**  
> Head turns back over the near shoulder; eyes meet the lens in a soft
> glance; mouth relaxed closed; neck long.

**Stride / sport**  
> Eyes along the path with a brief confident connection to lens; alert
> open lids; calm closed mouth; chin level.

**Product co-frame**  
> Eyes to the camera for connection while the product stays sharp in
> hand; natural closed-lip expression; soft beauty lids.

**Premium editorial (past camera)**  
> Gaze slightly past the camera (near-lens, not locked); open unhurried
> lids with catchlights; closed calm mouth; chin slightly turned;
> confident editorial presence without smile stack.

Hand near face only as **styling gesture** (brush hair, adjust earring,
hold product) — describe contact, not "sexy hand."

### 3.4 服装材质与剪裁 (stability foundation #1)

**最稳定、必写。** 专业时尚 / 运动 / 商业摄影都靠材质与剪裁成立；
不是靠 sexy 形容词。

Do **not** write only "nice clothes" / "fashion outfit". Every prompt
names **material hand + cut geometry + fit + opacity + coverage**.

#### Material menu (pick 1–2; state surface)

| Material | Prompt phrases (hand / behavior) |
|----------|----------------------------------|
| Silk / charmeuse | matte or low-sheen silk, soft fluid drape, light folds at the waist |
| Satin | structured satin, controlled highlight on planes, not wet-look |
| Technical satin | opaque technical satin (performance-smooth face, controlled sheen, not sheer couture organza) |
| Ribbed knit | fine rib stretch knit, vertical rib texture, body-skimming recovery |
| Cotton / poplin | brushed cotton or crisp poplin, matte, clean seams |
| Denim | mid-weight opaque denim, visible twill, structured hip |
| Wool / tailoring | pressed wool crepe or suiting, sharp crease, holds silhouette |
| Cashmere / jersey | soft cashmere loft or matte jersey, gentle cling without sheerness |
| Technical activewear | matte technical jersey or compression knit, stretch folds at joints |
| Leather / faux | soft matte or lightly pebbled leather, opaque panels |
| Linen | opaque linen weave, natural wrinkle, relaxed drape |

Always add: **fully opaque** · **no sheer** · **no wet-look transparency**.
Name sheen only as **matte** or **controlled / subtle sheen**.

#### Cut vocabulary (geometry + where it sits on the body)

Write cut as **shape on the body**, not mood:

| Zone | Concrete cut phrases |
|------|----------------------|
| Neckline | deep V to mid-sternum · scoop to collarbone · square neck · boat neck · crew · turtleneck · strapless straight across upper chest · off-shoulder cuff below collarbones · thin spaghetti straps · wide set-in straps |
| Bodice | darted tailored bodice · bias-cut slip · ribbed tube · structured bandeau with internal support · wrap front overlapping closed · princess seams |
| Waist | high-rise to natural waist · mid-rise · belt-cinched · elastic smocked waist · dropped waist |
| Hem / length | crop above navel · hip-length · tunic · mini above mid-thigh · tailored short · midi to mid-calf · ankle full-length · side slit to knee (leg only, still covered at seat) |
| Sleeve | sleeveless · cap · short set-in · bishop · long fitted · off-shoulder · one-shoulder |
| Bottom | high-waist cigarette pant · wide-leg tailored · A-line mini · pencil midi · bike short · full-length legging · denim short with clean hem |
| Sport cut | racerback · scoop sports top · high-waist compression legging · seven-eighth length |

#### Fit (how cloth meets body)

Pick one fit phrase per garment:

- **tailored** — shaped with darts/seams, close at waist, clean armhole  
- **body-skimming** — follows curve without compression, slight air gap  
- **structured** — holds its own shape (bandeau, blazer, satin shell)  
- **relaxed / ease** — intentional looseness at torso or hip  
- **compression** — activewear only; matte knit, still fully opaque  

#### Coverage lock (say what is covered)

Always close the outfit sentence with coverage:

> …fully covers the chest; high-waist [bottom] covers hips and seat;
> fabric fully opaque.

Example assembled outfit sentence:

> Opaque matte black silk camisole, bias-cut soft drape, thin adjustable
> straps, deep V neckline ending at mid-sternum, body-skimming fit,
> fully covering the chest; paired with high-rise opaque cream wool
> tailored shorts, clean hem at upper thigh.

Attraction = **cut + drape + light on fabric and collarbone**, not
undressing language.

### 3.5 身体朝向与重心 (stability foundation #2)

**最稳定、必写。** 与材质剪裁并列的另一条基座；证件照站姿会毁掉
时尚/运动/商业片的专业感。

Stiff passport pose kills attraction. Every full or ¾ figure prompt
must state **body orientation to camera** and **where weight sits**.

#### Body orientation (relative to camera)

| Orientation | Prompt phrases |
|-------------|----------------|
| Frontal | square to camera (use sparingly; beauty close-up only) |
| Slight ⅛–¼ turn | torso rotated ~15–30°; near shoulder closer to lens |
| Classic ¾ | torso ~45°; hip and shoulder line readable |
| Profile-leaning | body near profile, face turns back to lens (over-shoulder) |
| Walking angle | hips face travel direction; ribs/chest open slightly to camera |

Also specify **head vs torso** when they differ (lookbook essential):

- head toward camera, torso ¾ away  
- chin over near shoulder  
- face follows travel, eyes flick to lens  

#### Weight / 重心 (never “standing nicely”)

Name **loaded leg**, **free leg**, and hip result:

| Stance | Concrete weight phrases |
|--------|-------------------------|
| Contrapposto | weight on back leg; free front knee soft; loaded hip higher |
| Hip pop | weight on [left/right] leg; opposite hip drops slightly |
| Even athletic | weight stacked over mid-feet; ribs over pelvis; sport stance |
| Step / stride | weight transferring to front foot; back heel lifting |
| Lean | weight into [wall/stool/counter] through hip or forearm |
| Seated | weight on sit bones; feet planted or ankle cross; torso long |

**Hands and shoulders follow weight** (optional one line):

- free-side hand on hip; loaded-side arm relaxed  
- near shoulder slightly forward; far shoulder drops  
- for lean: contact point named (forearm on marble, hand on chair back)

#### Torso, hip, and leg line (attraction engines)

Write 1–2 of these when body is visible:

- soft S-curve: loaded hip out, free knee bent, ribs gently opposite  
- long-leg line: high-rise waist + free leg extended or crossed at ankle  
- open chest without stiff military shoulders  
- seated: knees together or elegant knee-over-knee; torso lean 10–15° to lens  

#### Anti-stiff block (avoid)

Do not leave pose as default: arms glued to sides, feet parallel
shoulder-width, hips and shoulders square, chin level mugshot.
If the user gave no pose, **insert** a ¾ turn + single-leg weight shift.

#### Pose sentence templates (paste and fill)

**Relaxed three-quarter + confident (mini-seed expanded)**  
> Relaxed three-quarter pose: torso about 45° to camera, weight on her
> back leg, front knee soft, shoulders relaxed, near shoulder slightly
> forward. Confident expression: open unhurried lids, closed calm mouth,
> chin slightly turned; [gaze to lens | slightly past camera].

**Standing lookbook**  
> Body at a three-quarter angle to camera; weight on her back leg, front
> knee soft; loaded hip slightly higher; near shoulder closer to lens;
> head turned to camera; one hand on the free-side hip.

**Over-shoulder**  
> Torso angled away about 45°, weight on the far leg; head and gaze turn
> back over the near shoulder toward the lens; neck long; shoulders relaxed.

**Activewear**  
> Mid-stride toward camera-left; weight on the front foot, back heel up;
> hips face the path; chest slightly open to camera; arms in natural run
> or recovery position.

**Seated commercial**  
> Seated on [surface], weight on sit bones, ankles crossed; torso leans
> slightly toward camera; shoulders soft; hands [on thigh / holding product].

### 3.6 镜头焦段 (stability foundation #4)

**最稳定、必写。** 专业时尚 / 运动 / 商业摄影用 **具体 mm + 景深 + 裁切**
建立透视与主体分离，不是 cinematic / bokeh / DSLR 空词。

Do **not** write only "cinematic" / "bokeh" / "professional photo" /
"shot on DSLR". Name a **focal-length feel** (full-frame equivalent) and
what it does to perspective and depth of field.

| Focal length | Prompt phrases | Best for |
|--------------|----------------|----------|
| **35mm** | 35mm environmental full-frame look; wider context; slight natural perspective | lifestyle, street editorial, activewear full body, location story |
| **50mm** | 50mm natural perspective; moderate background separation | lookbook ¾, editorial environment, balanced campaign |
| **85mm** | 85mm portrait telephoto compression; shallow depth of field; subject isolation | beauty hero, fashion face/torso, soft bokeh background |
| **105mm** | 105mm beauty/tele compression; tight facial planes; creamy background | beauty campaign close-up, skin detail, product-adjacent face |
| **24–28mm** | wide environmental (use sparingly) | architecture editorial, deliberate distortion at edges only if intentional |

**Always pair with DOF / focus intent:**

- shallow depth of field, eyes sharp, background soft bokeh  
- moderate DOF, face and torso both readable (lookbook garment)  
- deeper DOF for product + person both sharp (commercial)  

**Crop vs lens:** crop is framing (chest-up / mid-thigh / full-body);
lens is optical feel. Write both when possible:

> Mid-thigh fashion crop, 85mm shallow depth of field…

**Avoid:** stacking 5 different mm values; ultra-wide face distortion
for beauty; empty "DSLR photo" with no mm.

#### Genre → default lens (if user silent)

| Lane | Default package |
|------|-----------------|
| Beauty campaign | **85mm** shallow DOF, chest-up; eyes sharp |
| Fashion advertising / lookbook ¾ | **50mm** moderate DOF **or** **85mm** shallow, mid-thigh |
| Premium / magazine editorial | **35–50mm** environmental **or** **85mm** with negative space |
| Activewear full body | **35mm** (or 50mm) deeper/moderate DOF, full-body crop |
| Product co-frame | **50–85mm**, product + eyes planned sharp |
| Beauty extreme close | **105mm** shallow DOF, face planes |

#### Lens sentence templates

**Beauty**  
> Shot on 85mm (full-frame feel), shallow depth of field, eyes tack-sharp,
> soft background falloff, chest-up crop.

**Fashion campaign full / ¾**  
> 50mm natural perspective, moderate depth of field so garment texture
> stays readable, mid-thigh crop.

**Editorial location**  
> 35mm environmental framing, subject sharp, architecture and negative
> space readable behind her.

**Premium editorial hero**  
> Shot on 85mm, shallow depth of field, mid-thigh crop, soft background
> falloff, fashion compression on the figure.

**Product co-frame**  
> 85mm or 50mm, focus on product label with face still identity-sharp;
> controlled background blur.

**Activewear**  
> 35mm environmental full-body crop, moderate depth of field, path and
> figure both readable.

### 3.7 主光、补光和轮廓光 (stability foundation #5)

**最稳定、必写。** 专业时尚 / 运动 / 商业摄影靠 **主光塑形、补光控比、
轮廓分离**，不是 good lighting / cinematic light / sexy light。

Do **not** write only "good lighting" / "studio light" / "cinematic light"
/ "moody light". Describe a **three-point (or reduced) setup**: direction,
quality, and role for each light that is present — and **name absence**
when fill or rim is intentionally minimal/none.

#### Role definitions

| Light | Job | Must name |
|-------|-----|-----------|
| **主光 Key** | shapes face and body; sets mood | direction (camera-left/right, 45°, side, overhead-avoid), quality (soft/hard), source type |
| **补光 Fill** | lifts shadows; controls contrast | relative strength (gentle / weak / almost none), opposite side or bounce |
| **轮廓光 Rim / hair / kicker** | separates subject from background | from behind or ¾ back; on hair/shoulder edge; subtle vs bright |

Optional: **background light**, **practical** (window, neon) as key or
accent — still say which role it plays.

#### Quality & direction vocabulary

| Quality | Phrases |
|---------|---------|
| Soft key | large softbox key, window key, diffused 45° beauty key, broad soft source |
| Hard key | hard sunlight, bare bulb edge (editorial/sport), crisp shadow edge |
| Fill | gentle fill from camera-right, weak fill, silver bounce fill, clean bounce flash fill, almost no fill (high contrast) |
| Rim | subtle hair rim from behind camera-right, warm rim separating shoulders, cool edge light |

**Directions (pick camera-relative):**

- key at **45° camera-left**, slightly above eye line (beauty classic)  
- key **side light** camera-left (fashion / golden hour)  
- key **high butterfly** only if intentional beauty (watch under-nose shadow)  
- rim **from behind**, opposite the key when possible  

**Contrast recipes (genre defaults if user silent):**

| Mood | Key : Fill : Rim |
|------|------------------|
| Beauty commercial | soft 45° key · gentle fill · optional soft hair light |
| Fashion campaign | soft large softbox key · controlled fill · subtle rim |
| Premium editorial | soft window key · clean bounce flash fill · optional faint hair rim |
| Editorial drama | directional key · minimal/no fill · stronger rim or color edge |
| Activewear outdoor | sun as hard/side key · sky as soft fill · natural backlight rim |
| Product beauty | soft key on face+product · gentle fill · rim usually none/clean bg |
| Lookbook / e-comm | even soft key · mild fill · rim optional none |

**Catchlights:** when face is hero, key should create **soft catchlights
in both eyes** (usually from the key side). State it when beauty/editorial
face is primary.

#### Anti-light block

- flat overhead office light / ceiling-only  
- dual equal keys killing shape  
- pure black face shadow with no fill when selling beauty product  
- neon cyberpunk stack unless user asked  
- empty "cinematic lighting" with no key direction  

#### Lighting sentence templates

**Beauty three-point**  
> Soft large key at 45° camera-left slightly above eye line; gentle fill
> from camera-right; subtle hair rim from behind; soft catchlights in
> both eyes.

**Fashion campaign**  
> Large softbox key camera-left, controlled fill camera-right so shadow
> side still modeled; faint rim separating hair from background.

**Editorial high contrast**  
> Directional soft-hard key from camera-left; minimal fill; cool rim on
> the far shoulder and hair; intentional deep shadow on the far cheek.

**Window lifestyle**  
> Soft window key from camera-left as main light; weak interior bounce
> fill; no rim.

**Window + bounce flash (premium editorial)**  
> Soft window light as key from camera-left; clean bounce flash fill from
> camera-right (soft, not harsh on-camera look); optional faint hair
> separation; catchlights clean and natural.

**Sport outdoor**  
> Early morning sun as side key; open sky as soft fill; natural backlight
> rim on hair.

**Must-state rule:** every prompt includes at least **key** (direction +
quality + source). **Fill** and **rim** are either described or explicitly
`minimal fill` / `no rim`.

### 3.8 Style packages (genre + foundations combined)

When user is vague, pick **one** package (fashion = editorial **or** ad):

| Package | Style tag + lens + lights + face default |
|---------|------------------------------------------|
| **Fashion advertising** | fashion advertising campaign still · 50–85mm · softbox + controlled fill + subtle rim · campaign stare · mid-thigh |
| **Fashion editorial** | fashion magazine editorial · 35–50mm or 85mm · directional / window key + low or bounce fill + rim · past-camera or off-frame · negative space |
| **Premium editorial fashion** | premium editorial fashion photography · 50–85mm · window + bounce flash · past-camera · expand canonical seed |
| **Beauty ad** | beauty campaign · 85mm shallow · soft 45° + gentle fill · lens lock · chest-up |
| **Lookbook clean** | brand lookbook · 50–85mm · even soft key + mild fill · garment-readable DOF |
| **Activewear** | activewear campaign · 35–50mm · sun side + sky fill + rim · path gaze · full body |
| **Product lifestyle** | commercial product · 50–85mm · window key + soft fill · eyes to lens + product sharp |

#### Canonical seed — premium editorial fashion

Same full seed as **Stable foundation → Full premium editorial seed**
above. Always expand: identity + age + satin **cut/coverage** + mm/crop
if omitted. Do not emit the six short phrases alone as the whole prompt.

### 3.9 Word ban → rewrite table

Do **not** pad with: sexy, hot, seductive, erotic, NSFW, perfect body,
goddess, flawless, 勾人, 魅惑 (as empty tokens).

| Weak prompt | Rewrite into 造型 + craft |
|-------------|---------------------------|
| sexy woman | 26-year-old woman in opaque black silk deep-V camisole, soft 45° key + gentle fill, 85mm, ¾ gaze to lens |
| hot body | high-waist tailored shorts + fitted opaque knit, contrapposto, mid-thigh lookbook crop, 50mm |
| seductive eyes | soft direct eye contact; relaxed half-lidded eyes; closed-lip hint smile; catchlights; chin slightly down |
| 擦边 | structured opaque bandeau + high-waist bottom; key grazing collarbones; subtle rim; fashion campaign stance |
| cinematic / pro photo | name mm + key + fill + rim + **editorial XOR fashion ad** |
| fashion / stylish only | pick **editorial** or **fashion advertising** + matching defaults |
| beautiful girl | [age] + [style tag] + [朝向重心] + [材质剪裁] + [表情视线] + [焦段] + [三点光] |

Prefer: "85mm shallow DOF", "softbox key camera-left, gentle fill, hair rim",
"soft window light with clean bounce flash", "premium editorial fashion
photography", "gaze slightly past camera", "opaque technical satin",
"relaxed three-quarter pose" (expanded with weight on back leg),
"confident expression" (expanded with lids + mouth + gaze target),
"weight on back hip", "closed-lip hint smile".

## 4. Counts & anatomy sanity

State explicitly in the prompt:

- **People:** `exactly one woman` (or exact number if multi)
- **Hands:** `two hands only`, five fingers each when visible
- **Limbs:** two arms, two legs; no extra limbs or merged fingers
- **Face:** one face, two eyes

If hands interact with product/hair/face, describe the **contact point**
("right hand lightly touching the bottle neck", "left fingertips at
temple"). Floating props = fail; re-prompt with grip detail.

## 5. Pose recipes (pick one per shot; name the lane)

Use as skeleton; fill identity + **造型 slots** (§3.1) + scene:

1. **Beauty gaze lock:** soft direct to lens; half-lid + closed-lip hint
   smile; chin slightly down; ¾ face; grooming hand in hair; chest-up.
2. **Lookbook over-shoulder:** torso ¾ away; eyes meet lens over near
   shoulder; mouth relaxed closed; neck long; warm rim light.
3. **Leg-line lookbook:** weight on far leg; campaign stare or soft lens
   lock; high-waist bottom + crop/fitted top; mid-thigh crop.
4. **Activewear stride / recovery:** eyes along path or brief lens flick;
   alert open lids; chin level; opaque technical set; outdoor side light.
5. **Seated commercial lean:** soft lens lock or product glance; stool/
   sofa edge; torso lean in; hands on thigh or holding product.
6. **Product co-frame:** commercial; eyes to lens while product sharp;
   see §7.
7. **Premium editorial seed:** relaxed ¾ pose; confident expression;
   gaze slightly past camera; opaque technical satin + cut/coverage;
   soft window key + clean bounce flash fill; expand via §3.8.

Never default to arms-at-sides frontal mugshot.

## 6. Anti-artifact block (short, end of prompt)

Append once, lightly:

> Photorealistic, natural proportions, no extra fingers or limbs, no
> duplicate faces, no doll-face plastic skin, no wax-figure expression,
> no over-symmetry, opaque clothing only.

## 7. Woman + product co-frame

**Prerequisite:** §−1 capability / use-case brief is locked when the
product is AI/app/tool (or any marketing keyword set).

When product appears, **assign roles** in the prompt (and in multi-ref
`image_edit` if refs exist):

| Role | What to specify |
|------|-----------------|
| Person ref | face/identity locks only |
| Outfit ref | material, cut, color, opacity (if separate) |
| Product ref | brand-safe shape, label side, size relative to hand |
| **Capability on-screen** | UI state that **shows the confirmed capability** (e.g. before/after slider, inpaint brush, enhance button, remove-bg cutout) |
| **Use-case context** | props/set that sell the confirmed scenario (creator desk, e-comm table, travel laptop…) |
| Scene ref | room, light direction, surface material |

Prompt pattern:

> Exactly one 26-year-old woman [identity locks]. Capability brief:
> [primary capability]. Use case: [scenario]. She holds [device] with
> screen facing camera showing [UI state that proves the capability],
> fingers wrapped cleanly. Outfit: [opaque cut]. Setting: [use-case set].
> Soft window light from camera-left. Product/UI sharp and readable; face
> secondary or equal per hero hierarchy.

Hero hierarchy: say which is primary (face vs UI vs device). Do not let
hands obscure the capability UI unless intentional.

**Do not** invent a random AI feature (e.g. defaulting to “enhance”) when
the user has not confirmed — ask first (§−1).

## 8. Cross-image consistency workflow

1. Build identity card (§1).
2. Generate **one base hero** with `image_gen` (best face + outfit).
3. All variants via `image_edit` from that base (or nearest neighbor):
   "Keep this exact woman — same face, mole under left eye, hair,
   body proportions, skin tone — change only [pose / outfit / scene]."
4. Re-state the 4–6 high-signal locks in every edit prompt.
5. Verify set: same mole side, same eye color, same hair length/part,
   same age read. One mismatch → targeted re-edit of that frame only.

## 9. Prompt assembly order

**Before writing any prompt:** complete §−1 if product/AI/marketing.

Write prompts as natural prose in this order:

1. Count + age + gender (`exactly one 26-year-old adult woman`)
2. Identity locks (face, eyes, hair, mole, asymmetry)
3. **Capability brief + use case** (from user confirmation) — when product
4. **Style #0 — Genre:** fashion → **editorial OR fashion advertising**
   (never both vaguely); else beauty / activewear / lookbook / product
5. **Foundation A — 身体朝向与重心**
6. **Foundation B — 服装材质与剪裁**
7. **Foundation C — 表情与视线** (match genre defaults)
8. **Foundation D — 镜头焦段** (match genre defaults)
9. **Foundation E — 主光 / 补光 / 轮廓光** (match genre defaults)
10. Device / UI / props mapped to capability brief
11. Setting (use-case set + style set)
12. Texture cues + anti-artifact block

Target **6–10 sentences**. If short: **genre tag + A–E only** (plus
capability UI line when product). Never drop style choice, material/cut,
orientation/weight, expression (if face), mm/DOF, or named key lighting.

## 10. Aspect ratios

| Use | Ratio |
|-----|-------|
| Phone / Reels / story | `9:16` |
| Feed square | `1:1` |
| Banner / desktop hero | `16:9` |
| Editorial portrait | `3:4` or `4:5` |

Default social portrait: `3:4` or `9:16`.

## 11. Quick checklist before calling the tool

- [ ] Attraction via **造型** (north star), not sexy-token spam
- [ ] Register = professional fashion / sports / commercial (not sexy stack)
- [ ] **§−1:** if product/AI/keyword → capability and/or use case **confirmed** (or restated from user)
- [ ] On-screen UI/props match the confirmed capability (not a random feature)
- [ ] **Style #0:** if fashion → **editorial XOR advertising** named with tag phrase
- [ ] Concrete adult age (24–32) present
- [ ] Identity locks, not "beautiful woman"
- [ ] Gaze / light / lens match chosen style defaults
- [ ] **Foundation — 材质:** fiber + matte/sheen + opaque lock
- [ ] **Foundation — 剪裁:** neckline/bodice/waist/hem + coverage
- [ ] **Foundation — Fit** named
- [ ] **Foundation — 朝向:** body angle; head vs torso if split
- [ ] **Foundation — 重心:** loaded leg + free knee/hip (or seated)
- [ ] **Foundation — 视线:** named gaze target (lens / past camera / off-frame / product…)
- [ ] **Foundation — 表情:** lid energy + mouth shape + chin angle (if face visible)
- [ ] Catchlights if face is hero
- [ ] **Foundation — 焦段:** named mm (35/50/85/105…) + DOF intent + crop
- [ ] No empty "cinematic/bokeh/DSLR" without mm
- [ ] **Foundation — 主光:** direction + soft/hard + source
- [ ] **Foundation — 补光:** strength/side **or** explicit minimal/none
- [ ] **Foundation — 轮廓光:** rim/hair present **or** explicit none
- [ ] No empty "cinematic/good lighting" without roles
- [ ] Genre: editorial vs fashion ad when fashion-led
- [ ] No sexy/hot token spam; weak vibes rewritten (§3.9)
- [ ] Exactly N people, two hands, limb sanity
- [ ] If product: roles + grip + hero hierarchy
- [ ] If series: base image + edit chain planned
- [ ] Aspect ratio matches channel

## 12. Failure handling

- Moderation block → stop, do not paraphrase to evade; offer a more
  covered / less suggestive staging.
- Doll skin / wrong age-read → re-edit: strengthen pores + age number +
  under-eye texture; remove "cute/babyface" language.
- Extra fingers / twin faces → re-edit with explicit hand/face counts.
- Identity drift → re-edit from base with mole + hair + eye color first.
- Dead eyes / wrong gaze → re-edit: name gaze target + catchlights +
  one lid energy + mouth shape; remove vague "beautiful/seductive eyes".
- Flat/muddy light → re-edit: name key direction + fill strength + rim;
  remove empty "cinematic lighting".
- Wrong style read (e-comm vs editorial) → restate genre package from §3.2
  and matching lens/light contrast.

## Example starter prompts

**A — beauty campaign portrait (3:4)**  
Exactly one 27-year-old adult woman, soft oval face, warm hazel eyes,
shoulder-length dark brown waves with fine baby hairs, tiny freckle mole
under her left eye, right brow slightly higher. Beauty advertising still:
face in a slight three-quarter turn; soft direct eye contact with the
camera; relaxed half-lidded eyes with catchlights; lips gently closed
with a hint of a smile; chin slightly lowered. Clean dewy commercial
makeup; left fingertips lightly brush hair behind her ear as a grooming
gesture. Material/cut: opaque matte black silk camisole, thin straps,
deep V to mid-sternum fully covering the chest, body-skimming soft drape.
Shot on 85mm full-frame feel, shallow depth of field, chest-up crop.
Soft large key at 45° camera-left slightly above eye line; gentle fill
from camera-right; subtle hair rim from behind. Natural pores and
individual hair strands. Photorealistic, two hands only when visible,
no doll skin, no extra limbs.

**B — fashion advertising campaign long-leg (9:16)**  
Exactly one 25-year-old adult woman [same identity locks as series].
Fashion advertising campaign still: body at a three-quarter angle to
camera; weight on her back leg, front knee soft, loaded hip slightly
higher; near shoulder closer to lens; free-side hand on hip. Expression:
calm confident gaze straight into the lens; open but unhurried lids;
neutral campaign mouth. Material and cut: structured opaque champagne
satin bandeau with internal support, straight neckline across upper
chest, fully covering the chest, controlled satin sheen only; high-rise
opaque cream wool tailored shorts, clean hem at upper thigh, long-leg
line. 50mm natural perspective, moderate depth of field, mid-thigh crop.
Large softbox key camera-left; controlled fill camera-right; faint rim
separating hair from clean terrace bokeh. Natural skin texture, fully
opaque fabrics. Photoreal, correct anatomy, no duplicate figures.

**C — fashion magazine editorial (3:4)**  
Exactly one 26-year-old adult woman [identity locks]. Fashion magazine
editorial portrait: torso ~45° to camera; weight on far leg; head in a
cool three-quarter turn. Gaze slightly off-frame camera-right; slightly
narrowed editorial lids; neutral mouth; no smile. Outfit: opaque black
wool tailored blazer, strong shoulder, closed front fully covering the
chest, matte fabric. 35mm environmental feel, intentional negative space,
architecture soft behind. Directional key from camera-left; minimal fill
for deeper shadow; cool rim on far shoulder and hair. High-fashion
editorial contrast, photoreal, correct anatomy.

**D — commercial product co-frame**  
Exactly one 28-year-old adult woman [identity locks]. Beauty/lifestyle
ad: frosted glass serum bottle in her right hand at lower-third, label
facing camera, fingers wrapped cleanly. Eyes to the camera; soft beauty
lids with catchlights; natural closed-lip expression; chin slightly
lowered. Outfit: opaque off-shoulder dusty-rose ribbed knit, folded cuff
below collarbones, body-skimming, fully covering the chest. 85mm, product
and eyes planned sharp, background softly blurred. Soft window key
camera-left as main; weak bounce fill; no hard rim. Clean marble set.
No extra hands, no see-through fabric.

**E — activewear campaign (9:16)**  
Exactly one 26-year-old adult woman [identity locks]. Outdoor activewear
campaign: mid-stride freeze toward camera-left; weight on front foot,
back heel up. Eyes along the path with a brief confident connection to
lens; alert open lids; calm closed mouth; chin level. Opaque matte
technical jersey set — fitted cropped sports top with scoop neck fully
covering the chest + high-waist full-length leggings. 35mm environmental
full-body crop. Early sun as side key; open sky as soft fill; natural
backlight rim on hair. Photoreal, two hands, correct anatomy, fully opaque.

**F — premium editorial fashion (seed expanded, 3:4)**  
Exactly one 26-year-old adult woman [identity locks]. Premium editorial
fashion photography. Relaxed three-quarter pose: torso about 45° to
camera, weight on her back leg, front knee soft, shoulders relaxed, free
hand lightly at the seam of her hip. Confident expression: open unhurried
lids, closed calm mouth, chin slightly turned; gaze slightly past the
camera (near-lens, not locked). Outfit: opaque technical satin column
dress, clean crew neckline, body-skimming tailored fit, controlled sheen
only, fully covering the chest and body to mid-calf. Soft window light as
key from camera-left; clean bounce flash fill from camera-right; shot on
85mm, shallow depth of field, mid-thigh crop. Natural pores, technical
satin surface detail, individual hair strands. Photoreal, two hands, no
doll skin, fully opaque clothing.
