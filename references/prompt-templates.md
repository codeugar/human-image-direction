# Prompt templates — human-image-direction

Fill brackets. Keep identity locks identical across a series.

**语域:** 专业时尚 / 运动 / 商业摄影 — **不是**堆叠 sexy。  
**North star:** 用造型表达吸引力。  
**更稳定的方式:** 具体描述，尤其是下面 **Foundation**。  
**产品/AI 出图:** 先确认 **核心能力 or use case**（§−1），再写 prompt / 出图。

## §−1 能力 / 用例确认（产品关键词时必问）

在调用 `image_gen` 前向用户确认（可多选排序）：

```
出图前确认主打什么：
1) 核心能力 — 例如：一键增强 / inpaint 局部重绘 / 去背景 / 前后对比 / 文生图 / 放大…
2) Use case — 例如：博主修自拍 / 电商修产品图 / 设计师改海报 / 旅行照换天…
另：英雄主体 = 人像 | 屏幕 UI | 设备？  画幅 = 9:16 | 3:4 | 16:9？
```

用户已写清能力+用例 → 复述一行 brief 后直接生成。  
只给关键词（如 “AI image editor”）→ **必须先问**，禁止默认瞎选功能。

```
Capability brief: primary={...}; secondary={... optional}
Use case: {...}
Hero: {face|UI|device}
```

## Foundation（必写，最稳 — 风格 #0 + 五条）

### 0. 编辑摄影 or 时尚广告（时装必选其一）

```
# Editorial
fashion magazine editorial / premium editorial fashion photography

# Fashion advertising
fashion advertising campaign still / luxury brand campaign
```

| | Editorial | Fashion advertising |
|--|-----------|---------------------|
| Goal | mood / story | brand / aspiration |
| Gaze | past camera / off-frame | campaign stare / lens lock |
| Light | directional · low fill · stronger rim | softbox · controlled fill · subtle rim |
| Lens | 35–50mm or 85mm + negative space | 50–85mm mid-thigh hero |
| Set | architecture / texture / sparse | seamless / premium clean |

Vague "fashion" only → default **fashion advertising**; use editorial when
user wants 杂志/大片/情绪.

### 1. 服装材质与剪裁

```
opaque {material + hand}, {matte|controlled sheen}, fully opaque;
{neckline geometry ending at X}; {fit}; fully covering the chest;
{high-rise bottom + hem}, covers hips and seat.
```

### 2. 身体朝向和重心

```
Body at a {¾ ~45°|15–30°|over-shoulder} angle to camera;
weight on {back|left|right|front} leg, free knee {soft|extended};
{loaded hip higher|near shoulder forward}; head {to lens|turned}.
```

#### Mini-seed: `relaxed three-quarter pose` → expand

```
Relaxed three-quarter pose: torso about 45° to camera, weight on her back
leg, front knee soft, shoulders relaxed, near shoulder slightly forward
{, free-side hand on hip}.
```

### 3. 表情与视线

```
Gaze: {soft direct to lens | slightly past camera | off-frame | over-shoulder | path | product}.
Lids: {half-lid soft | bright open | alert athletic | narrow editorial}
  {+ catchlights if face hero}.
Mouth: {closed calm | closed-lip hint smile | soft parted | neutral editorial}.
Chin: {level | slightly lowered | slight ¾ turn}.
```

#### Mini-seed: `confident expression` → expand (gaze still required)

```
Confident expression: open unhurried lids with catchlights, closed calm
mouth (or closed-lip hint smile), chin slightly turned;
{calm confident gaze to the lens | gaze slightly past the camera}.
```

| Style | Gaze after "confident expression" |
|-------|-------------------------------------|
| Fashion advertising | calm confident gaze to the lens |
| Editorial | gaze slightly past the camera |
| Beauty | soft direct eye contact with the camera |

#### Combined mini-seed line

```
relaxed three-quarter pose, confident expression,
```
→ always expand to full pose block + full face block (never leave as-is).

#### Full premium editorial seed (complete)

```
relaxed three-quarter pose, confident expression,
gaze slightly past camera, opaque technical satin outfit,
soft window light with clean bounce flash,
premium editorial fashion photography
```

| Phrase | Expand |
|--------|--------|
| relaxed three-quarter pose | 45° torso; weight on back leg; soft front knee; relaxed shoulders |
| confident expression | open unhurried lids; closed calm mouth; chin slightly turned |
| gaze slightly past camera | near-lens not locked; both eyes same way; + catchlights |
| opaque technical satin outfit | fully opaque technical satin + **must add cut/coverage** |
| soft window light with clean bounce flash | window key (side) + bounce flash fill (opposite); rim optional |
| premium editorial fashion photography | Style #0 = editorial (not fashion advertising) |

**Default if mm missing:** 85mm shallow, mid-thigh.

**Full expanded (editorial):**

```
{IDENTITY}. Premium editorial fashion photography. Relaxed three-quarter
pose: torso about 45° to camera, weight on back leg, front knee soft,
shoulders relaxed. Confident expression: open unhurried lids with
catchlights, closed calm mouth, chin slightly turned; gaze slightly past
the camera (near-lens, not locked). Outfit: opaque technical satin
{neckline + fit + fully covering chest + hem}, controlled sheen only.
Soft window key camera-left; clean bounce flash fill camera-right;
optional faint hair rim. Shot on 85mm, shallow DOF, mid-thigh crop.
Natural pores, satin surface detail. Photoreal, two hands, opaque only.
```

### 4. 镜头焦段

```
Shot on {35|50|85|105}mm full-frame feel, {shallow|moderate|deeper} depth of
field, {chest-up|mid-thigh|full-body} crop.
```

| Lane | Default |
|------|---------|
| Beauty | 85mm shallow, chest-up |
| Fashion ad / lookbook | 50mm moderate or 85mm shallow, mid-thigh |
| Editorial location | 35–50mm environmental |
| Activewear | 35mm full-body |
| Product | 50–85mm, product+eyes sharp |
| Beauty tight | 105mm shallow |

禁止：只写 cinematic / bokeh / DSLR。

### 5. 主光、补光和轮廓光

```
Key: {softbox|window|sun|beauty dish} at {45° camera-left|side|...}, {soft|hard}.
Fill: {gentle opposite|clean bounce flash|weak sky|minimal|none}.
Rim: {subtle hair rim from behind|cool edge on far shoulder|none}.
{+ soft catchlights in both eyes if face hero}
```

| Lane | Key : Fill : Rim |
|------|------------------|
| Beauty | soft 45° key · gentle fill · optional hair light |
| Fashion ad | large softbox · controlled fill · subtle rim |
| Premium editorial | window key · bounce flash fill · optional faint hair rim |
| Editorial drama | directional key · minimal fill · stronger rim |
| Activewear | sun side key · sky fill · backlight rim |
| Product | soft key · gentle fill · usually no rim |

禁止：只写 good lighting / cinematic light / studio light。

### 基座：风格 + 五合一

```
{fashion magazine editorial | fashion advertising campaign still}.
{朝向+重心句}. {表情+视线句}. Outfit: {材质+剪裁句}.
Shot on {mm}, {DOF}, {crop}.
Key: {...}. Fill: {...}. Rim: {...}.
```

### Editorial full seed

```
{IDENTITY}. Fashion magazine editorial. Body at three-quarter angle; weight
on back leg, front knee soft. Gaze slightly past the camera; open unhurried
lids; closed calm mouth; chin slightly turned. Outfit: opaque {material+cut
+coverage}. Shot on 50mm, moderate DOF, mid-thigh with negative space.
Soft window key camera-left; clean bounce flash fill; faint hair rim.
Photoreal, two hands, opaque clothing only.
```

### Fashion advertising full seed

```
{IDENTITY}. Fashion advertising campaign still. Body at three-quarter angle;
weight on back leg, front knee soft; free-side hand on hip. Calm confident
gaze to lens; open unhurried lids; closed calm or hint smile. Outfit: opaque
{material+cut+coverage}. Shot on 85mm, shallow DOF, mid-thigh crop. Large
softbox key camera-left; controlled fill camera-right; subtle hair rim.
Photoreal, two hands, opaque clothing only.
```

### 表情与视线 — 语域默认

| Lane | Gaze + face |
|------|-------------|
| Beauty | soft direct to lens; half-lid or bright open; closed-lip hint smile; chin slightly down |
| Fashion ad | campaign stare to lens; open unhurried lids; neutral or hint smile |
| Premium editorial | slightly past camera or off-frame; closed calm mouth; chin slight turn |
| Activewear | along path or brief lens flick; alert open lids; chin level |
| Product | eyes to lens (connection) while product sharp; closed calm mouth |

## 造型 → 吸引力 (quick rewrite)

| User said | Write instead |
|-----------|----------------|
| 好看 / attractive | identity + **材质剪裁** + **¾+重心** + soft key |
| 性感 / sexy | high-waist long-leg OR deep-V **opaque cut** + **weight on back leg** + collarbone light |
| 擦边 | structured opaque cut + coverage + **three-quarter stance** + shape light |
| 吸引人 | foundation 两条 + premium editorial / fashion-ad package |

**Seed shape:** [editorial XOR fashion ad] · [朝向+重心] · [opaque 材质+剪裁] · [gaze] · [mm+DOF+crop] · [key+fill+rim]

## Identity lock block (paste every time)

```
exactly one {AGE}-year-old adult woman, {FACE_SHAPE} face, {EYE_COLOR} eyes,
{HAIR}, tiny freckle mole under her left eye, right eyebrow slightly higher than left
```

## 造型 slot fill-in (required core)

Before generating, complete at least:

```
Genre: {fashion editorial | fashion advertising campaign | lookbook | beauty campaign | activewear | lifestyle ad}

# 服装材质与剪裁
Material + hand: {matte silk drape | ribbed knit | technical jersey | wool crepe | ...}
Sheen: {matte | controlled subtle sheen}, fully opaque, no sheer
Cut geometry: {neckline + bodice + waist rise + hem length}
Fit: {tailored | body-skimming | structured | relaxed | compression}
Coverage: fully covers chest; bottom covers hips/seat as applicable
Silhouette (optional): {cropped + high-waist long-leg | column | athletic set}

# 身体朝向与重心
Body angle to camera: {frontal | ~15–30° | ¾ ~45° | over-shoulder}
Head vs torso: {same direction | head to lens / torso away}
Weight / 重心: weight on {back/left/right/front} leg; free knee {soft/extended}
Hip/shoulder: {loaded hip higher | near shoulder forward}

# 表情与视线
Gaze target: {soft direct to lens | campaign stare | over-shoulder glance | off-frame | path | product}
Lid energy: {half-lid soft | bright open | alert athletic | narrow editorial}
Mouth: {closed calm | closed-lip hint smile | soft parted | neutral editorial}
Chin: {level | slightly lowered | slight ¾ turn}
Catchlights: soft catchlights in both eyes (if face hero)

# 镜头焦段
Lens: {35mm environmental | 50mm natural | 85mm portrait | 105mm beauty}
DOF: {shallow eyes-sharp | moderate garment-readable | deeper product+face}
Crop: {chest-up | mid-thigh lookbook | full-body hero}

# 主光 / 补光 / 轮廓光
Key: {softbox/window/sun} at {45° camera-left | side | ...}, {soft|hard}
Fill: {gentle opposite | weak bounce | minimal/none}
Rim: {subtle hair rim from behind | cool edge | none}
```

## Expression + gaze line (paste when face is visible)

```
{Gaze target}. {Lid energy} with catchlights; {mouth}; chin {angle}.
```

## Lens + light line (Foundation D + E — paste every time)

```
Shot on {35|50|85|105}mm full-frame feel, {shallow|moderate|deeper} depth of
field, {crop}. Key: {source + direction + soft/hard}. Fill: {strength + side
| minimal | none}. Rim: {hair/shoulder edge from behind | none}.
```

## Style packages (pick one)

```
# Beauty ad
beauty advertising; 85mm shallow DOF; soft 45° key + gentle fill + optional hair light

# Fashion advertising
fashion advertising campaign still; 50–85mm; softbox key + controlled fill + subtle rim

# Fashion editorial
fashion magazine editorial; 35–50mm; directional key + minimal fill + rim; negative space

# Premium editorial fashion (canonical seed — always expand)
relaxed three-quarter pose, confident expression,
gaze slightly past camera, opaque technical satin outfit,
soft window light with clean bounce flash,
premium editorial fashion photography
# → expand: identity+age, weight/leg, lid+mouth, cut+coverage, 50–85mm, window=window fill=bounce flash

# Activewear
outdoor activewear campaign; 35–50mm full body; sun side key + sky fill + backlight rim
```

## Premium editorial fashion (full expanded)

```
{IDENTITY}. Premium editorial fashion photography. Relaxed three-quarter pose:
torso ~45° to camera, weight on back leg, front knee soft, shoulders relaxed.
Confident expression: open unhurried lids, closed calm mouth, chin slightly
turned; gaze slightly past the camera (near-lens, not locked). Outfit: opaque
technical satin {cut + fit + coverage}, controlled sheen, fully covering the
chest. Soft window key camera-left; clean bounce flash fill camera-right;
shot on 50–85mm, {crop}. Natural pores and satin surface detail. Photoreal,
two hands, no doll skin, opaque clothing only.
```

## Standing: orientation + weight line (paste into full-body)

```
Body at a three-quarter angle to camera; weight on her back leg, front knee
soft; loaded hip slightly higher; near shoulder closer to the lens; head
turned to camera; free-side hand on hip.
```

## Material + cut line (paste into outfit)

```
{Material hand}, fully opaque; {neckline geometry ending at X}; {fit} through
the torso; fully covering the chest; {high-rise bottom + hem length}, covers
hips and seat.
```

## Beauty campaign portrait (3:4)

```
{IDENTITY}. Beauty advertising still: face in a slight three-quarter turn; soft
direct eye contact with the camera; relaxed half-lidded eyes with catchlights;
lips gently closed with a hint of a smile; chin slightly lowered. Clean dewy
commercial makeup. Left fingertips brush hair behind the ear (grooming
gesture). Outfit: {OPAQUE — material, cut, fit, coverage}. Shot on 85mm,
shallow DOF, chest-up. Soft large key at 45° camera-left; gentle fill
camera-right; subtle hair rim from behind. Natural pores, hair strands.
Photorealistic, no doll-face plastic skin, no extra limbs, opaque clothing only.
```

## Fashion advertising long-leg (9:16)

```
{IDENTITY}. Fashion advertising campaign still: body at three-quarter angle;
weight on back leg, front knee soft, loaded hip slightly higher; free-side hand
on hip; calm confident gaze to lens. Material/cut: {OPAQUE_TOP} + high-rise
{OPAQUE_BOTTOM}. 50mm natural perspective, moderate DOF, mid-thigh crop. Large
softbox key camera-left; controlled fill camera-right; faint hair rim. Fully
opaque fabrics. Photoreal, two hands, no extra limbs.
```

## Fashion magazine editorial (3:4)

```
{IDENTITY}. Fashion magazine editorial: torso ~45°; cool three-quarter head turn;
gaze slightly off-frame; narrowed editorial lids; neutral mouth. Outfit:
{OPAQUE tailored pieces}. 35mm environmental feel, intentional negative space.
Directional key camera-left; minimal fill; cool rim on far shoulder and hair.
High-contrast art-directed editorial, photoreal, correct anatomy.
```

## Fashion lookbook over-shoulder (3:4 or 9:16)

```
{IDENTITY}. Studio or location lookbook: body angled three-quarters away, head
turned back to the lens (over-shoulder turn), long neck, calm campaign
expression. Outfit styling: {OPAQUE — silhouette, material, neckline, coverage}.
85mm shallow DOF. Softbox key on face; warm rim on hair. Natural skin
micro-texture and flyaway strands. Photoreal fashion still, correct anatomy.
```

## Activewear campaign (9:16)

```
{IDENTITY}. Outdoor activewear campaign: {mid-stride freeze | hands on hips
recovery}; eyes along path or brief lens connection; alert open lids. Opaque
matte technical set: {sports top with coverage} + {high-waist legging}. 35mm
full-body crop. Early sun as side key; open sky as soft fill; natural backlight
rim on hair. Photoreal, correct anatomy, fully opaque.
```

## Structured bandeau as fashion cut (still clothed)

```
{IDENTITY}. {POSE with lookbook language}. She wears a structured opaque satin
bandeau with internal support, straight neckline across the upper chest, fully
covering the chest, clean fashion finish, no straps; paired with {OPAQUE_BOTTOM
+ rise/length for silhouette}. Soft directional light grazing collarbones.
No transparency. Photoreal natural pores and hair strands.
```

## Commercial seated + product

```
{IDENTITY}. Lifestyle/beauty ad: seated on {SURFACE}, torso leaning slightly
toward camera, knees together or elegantly crossed. Right hand holds {PRODUCT:
shape, material, label facing camera} with fingers wrapped on the body; left
hand rests on her thigh. Outfit styling: {OPAQUE — material, cut, fit, coverage};
soft neutral grooming. Soft window key camera-left. Product readable; face
identity-locked. Exactly one person, two hands only.
```

## Weak vibe → 造型 rewrite (keep next to drafts)

| Avoid | Prefer |
|-------|--------|
| sexy / hot / 勾人 | name silhouette + fabric + light + pose |
| seductive / 迷人眼神 | soft direct to lens; half-lid; closed-lip hint smile; catchlights; chin slightly down |
| perfect body | high-waist rise, contrapposto, mid-thigh crop |
| 擦边 only | structured opaque cut + collarbone light + lookbook stance |
| blank pretty face | named gaze target + one lid energy + one mouth shape |
| cinematic / pro lighting | 85mm + soft 45° key + gentle fill + hair rim + genre |
| fashion vibe only | pick editorial vs fashion advertising + matching contrast |

## Edit-chain keep line

```
Keep this exact woman — same face shape, same warm eye color, same dark brown
wavy hair and part, same freckle mole under her left eye, same skin tone and
body proportions. Change only: {DELTA — pose / 造型 / scene / product}. Maintain
opaque clothing, named fabric hand, and photoreal natural skin texture. Keep
the same genre lane: {fashion | sport | commercial beauty}.
```
