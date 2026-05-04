# 05. Template Engineering и Best Practices AI-генерации видео для fashion

> **Дата сборки:** 4 мая 2026
> **Цель блока:** дать архитектуре «Фабрики Контента» рабочий контракт шаблона видео + промпт-стратегию для Veo 3.1 / Sora 2 / Kling 3 / Nano Banana / Higgsfield / Midjourney v7. Только факты с источниками + конкретные примеры promptов и YAML-схема, которую можно сразу втащить в код.
> **Источники:** официальные docs (OpenAI Cookbook, Google Vertex AI, Higgsfield), сайты Botika / Veesual / Resleeve / Flair / Arcads / Creatify / Topview, гайды LTX Studio / DreamHost / Skywork / Invideo, обзоры Atlas Cloud / aifreeapi / costgoat / arsturn (45+ источников).

## Содержание

1. [Архитектура шаблонов у конкурентов: что копировать](#sec-1-competitor-architectures)
2. [Промпт-инжиниринг по моделям (Veo 3.1, Sora 2, Kling 3, Higgsfield, Nano Banana, Midjourney v7)](#sec-2-model-prompts)
3. [Топ-5 best-practice prompts для fashion-видео ≤30s](#sec-3-best-prompts)
4. [Подвохи и пять главных fail-зон AI-генерации одежды](#sec-4-pitfalls)
5. [Параметрическая модель шаблона «Фабрики Контента» (YAML)](#sec-5-yaml-schema)
6. [Prompt-Builder: как YAML превращается в текст для Veo / Sora](#sec-6-prompt-builder)
7. [Cost & throughput benchmarks (Veo / Sora / альтернативы)](#sec-7-benchmarks)
8. [Источники](#sec-8-sources)

---

<a id="sec-1-competitor-architectures"></a>
## 1. Архитектура шаблонов у конкурентов

### 1.1 Сводная таблица — кто как структурирует шаблон

| Сервис | Уровень параметризации | Что задаёт пользователь | Что выбирается из библиотеки | Доступ к prompt level | Best for copy |
|---|---|---|---|---|---|
| **Botika** | Средний | Photo flat-lay, выбор модели, фон | 60+ моделей (этничность/возраст/телосложение), фоны, позы | нет — скрыт | Каталог моделей + кредитная экономика (5 кр / видео) |
| **Veesual** | Высокий (B2B) | Каталог товаров + размерная сетка | Моделей, цвета фона, поза | нет — VITON-GAN, не текстовый prompt | «Switch Model» / «Mix & Match» as UX-паттерн |
| **Resleeve** | Низкий-средний | Sketch / CAD / flat-lay | Стиль модели, lookbook-сцены, runway | нет — закрытая студия + люди | Концепт «команда сопровождает шаблон 24h» |
| **Flair.ai** | Высокий | Drag-and-drop canvas + текстовый prompt | Шаблоны e-com (cosmetics, fashion, food), модели, фоны | да — частично (text prompt overlay) | **Шаблон как переиспользуемый канвас** |
| **Arcads** | Высокий (UGC) | Скрипт + выбор актёра | 300+ актёров (демографика, тон) | нет (lip-sync engine) | UGC-шаблоны = актёр+скрипт+рамка |
| **Creatify** | Высокий | Heading, Sub, Color, Music, Avatar | Категории (Beverages/Beauty/OTC), сцены, музыка | частично (timeline edit per scene) | **Категория → шаблон → 4 поля → видео** |
| **Topview** | Высокий | Reference video + product photo | Sci-Fi/UGC/Demo шаблоны, аватары, music | да (script generation visible) | «AI Video Agent» — анализ референса + копирование структуры |
| **AdCreative.ai** | Средний | Brand kit + сценарный шаблон | 1500+ ad templates по категориям | нет | Tier-shaped pricing $39–599 |
| **Pictory / Invideo** | Низкий | Текст / URL / blog | Готовые шаблоны структуры (intro/body/CTA) | частично | Storytelling-структура с типовыми сценами |

### 1.2 Топ-3 архитектуры для копирования

**Top-1 — Creatify (категория → шаблон → 4–6 полей).** Самая чистая UX: пользователь выбирает категорию (Beauty / Beverages / OTC + «coming soon» по запросу аудитории), внутри неё — 5–10 готовых шаблонов с разной композицией/тоном, дальше заполняет минимальные поля Heading + Subhead + Product image + Color/Music. Экспорт в 9:16 / 16:9 / 1:1. Эта схема укладывается в нашу логику «1 SKU клиента → 1 видео под платформу» без обучения пользователя промптам. ([Creatify templates docs](https://creatify.ai/blog/product-to-video-templates-pre-built-ad-formats-for-instant-campaigns))

**Top-2 — Flair.ai (canvas + named scene + reusable template).** Drag-and-drop + сохраняемые «branded templates» = pixel-level контроль композиции при сохранении переиспользуемости. Для fashion: Flair поддерживает «model preset» (body type / skin tone / age / pose / setting) — ровно то, что нам нужно как параметризуемый слой над generic шаблоном. ([Flair AI fashion models page](https://flair.ai/product-category-features/fashion-models))

**Top-3 — Topview (Reference Video Agent → копирование hook+pacing+structure).** Topview анализирует чужое viral-видео (style, pacing, hooks, shot structure) и генерит шаблон. Это даёт нам способ кормить «Фабрику Контента» библиотекой выученных шаблонов из ТикТок-трендов: парсим референс → JSON → используем как базу для нового шаблона. ([Topview AI Video Agent](https://www.topview.ai/guides/video-agent), [WithLore breakdown](https://www.withlore.co/blog/topview-ai-viral-video-agent/))

**Что НЕ копировать.** Botika/Veesual закрывают prompt-level и фиксируют свой VITON/GAN-движок — нам это не подходит, так как мы хотим открытый prompt-builder поверх Veo/Sora/Kling. Resleeve пока строит «AI + люди в петле» (24h turnaround через дизайнеров) — для масштабирования 1550 видео/мес не годится. Arcads хорош для UGC-актёр+скрипт, но фиксирует видео в формате говорящей головы — это ортогонально fashion-каталог-видео. ([Botika products](https://botika.com/products), [Veesual VITON-GAN](https://www.veesual.ai/blog/virtual-try-on-styling-fitting), [Resleeve studio](https://resleeve.ai/), [Arcads workflow](https://www.codeitbro.com/blog/arcads-ai-review))

### 1.3 Общие паттерны (что присутствует у 6+ из 9 сервисов)

- **Каталог моделей** (этничность × возраст × телосложение × поза) — Botika, Flair, Veesual, Resleeve, Arcads, Topview.
- **Категория-first навигация** — Creatify, AdCreative, Topview, Flair, Pictory.
- **Шаблон сохраняется как «branded template»** для повторного использования — Creatify, Flair, Topview, AdCreative.
- **Многоформатный экспорт** 9:16 / 16:9 / 1:1 в одну кнопку — все 9 сервисов.
- **Batch/A-B variations** (несколько вариантов из одного шаблона) — Creatify, Topview, Arcads.

> Вывод для архитектуры: наш YAML должен поддерживать (а) категории, (б) переиспользуемые компоненты (model_preset, location_preset, music_preset), (в) тонкий слой параметров клиента (product, copy, color), (г) batch-генерацию N вариантов из одного template_id.

---

<a id="sec-2-model-prompts"></a>
## 2. Промпт-инжиниринг по моделям

### 2.1 Veo 3.1 (Google)

**Структура prompt'а — 7-слойная формула (Invideo/Veo guide).**

1. **Camera Move + Lens** (`Medium close-up on 35mm lens, locked at eye level`)
2. **Subject Specification** (стабильные визуальные признаки: возраст, этничность, волосы, одежда)
3. **Action & Physics** (`leaning slightly forward, then relaxing back`)
4. **Setting + Atmosphere** (`quiet office, blurred monitors glowing faintly`)
5. **Light Source** (направленный, не просто «bright»: `soft daylight from a side window`)
6. **Style/Texture** (микро-детали: `fine skin pores, visible fabric weave, subtle contrast`)
7. **Audio** (диалог + ambient + SFX)

**Длина sweet-spot.** 75–125 слов (Invideo, story321, ltx.studio). Veo 3.1 clip = 8 сек максимум, для длинного видео — chain через `Extend`. ([Invideo Veo guide](https://invideo.io/blog/google-veo-prompt-guide/), [story321 playbook](https://story321.com/blog/veo-31-prompt))

**Что критично для одежды.**
- `visible fabric weave`, `cotton hoodie`, `pleated satin pants` — называть материал явно.
- Действия типа `fabric lifts from fan`, `jacket opens during pose`, `accessories catch light` — явно описывать взаимодействие ткани и движения.
- Light source перед стилем: «soft 3-point studio lighting» даёт стабильный fit, неявное `bright` — даёт АИ-слоп.

**Negative prompts — рабочая стратегия.** Не использовать `no/don't` (Vertex AI docs прямо запрещают «instructive language»). Вместо `no walls` писать `wall, frame` в отдельный negative-список. Для рук/лица — `consistent anatomy, natural hand pose` в позитивном виде, а в negative — `distorted hands, extra limbs, morphing, duplicate limbs, motion blur`. ([Vertex AI Veo prompt guide](https://docs.cloud.google.com/vertex-ai/generative-ai/docs/video/video-gen-prompt-guide), [Skywork mistakes](https://skywork.ai/blog/veo-3-1-prompting-mistakes-fixes/))

**Для текста на экране.** Veo 3 плохо рисует subtitles → вместо кавычек `"My name is Ben"` писать через двоеточие `A guy says: My name is Ben`, добавлять `(no subtitles)` в prompt. ([snubroot Veo-3-Prompting-Guide](https://github.com/snubroot/Veo-3-Prompting-Guide))

**Workaround для камеры.** Точная фраза `"(that's where the camera is)"` после описания шота заметно повышает корректность viewpoint (snubroot guide).

### 2.2 Sora 2 (OpenAI)

**Структура (OpenAI Cookbook official).**

1. **Style/Aesthetic** (`1970s film`, `editorial fashion reel`, `Technicolor Noir`)
2. **Scene Description** (плотная проза: персонажи, сеттинг, props, атмосфера)
3. **Cinematography** (framing + lens + DOF + angle)
4. **Lighting & Palette** (источники света + color anchors)
5. **Actions** — нумерованные beats или счёт шагов (`takes four steps`, не `walks across`)
6. **Dialogue** — отдельный блок ниже описания, спикеры через `—`, alternating turns
7. **Sound/Audio** — diegetic + ambient cues

**Длина.** 2–3 предложения = больше creative variation. Подробный breakdown = больше control. На fashion рекомендуется длинный prompt с шот-листом по beats. ([OpenAI Sora 2 cookbook](https://developers.openai.com/cookbook/examples/sora/sora2_prompting_guide), [Higgsfield Sora 2 guide](https://higgsfield.ai/sora-2-prompt-guide))

**Длительность клипа.** 4 / 8 / 12 сек (durаtion tiers). 4s = 1–2 dialogue exchanges, 8s = несколько, 12s = полноценный шот-лист.

**JSON storyboard format (Sora 2 native).**

```json
{
  "model": "sora-2-pro",
  "size": "720x1280",
  "seconds": "12",
  "storyboard": true,
  "style": "Editorial Fashion Reel",
  "scenes": [
    {
      "timestamp": "0-2s",
      "description": "...",
      "camera": {"lens": "35mm", "movement": "dolly in"},
      "lighting": {"key": "strobe burst", "fill": "tungsten side"},
      "transition": "match cut"
    }
  ],
  "characters": [{"id": "char_123", "reference": "..."}]
}
```

([Sora 2 JSON template guide](https://solvingtools.github.io/JSON-Prompt-Gen/src/blog/sora-json-prompt-guide/index.html), [GitHub sora-prompt-generator](https://github.com/shijincai/sora-prompt-generator))

**Что критично для одежды (Higgsfield Sora 2 fashion guide).**
- Лензы: чередовать **35mm (wide tracking)** и **85mm (macro close-ups)** + shallow DOF.
- Light: **strobe bursts + constant LED fill + tungsten side spill**.
- Color anchors: `matte black, metallic gold, ivory, deep shadow blue, neutral gray`.
- Pacing: для 12s reel — `2-2-2-2-2-2 beat rhythm` (7 cuts), каждый beat = одна камера + одно действие.
- Garment language: `asymmetric black leather jacket`, `high-waisted pleated satin pants`, `gold statement earrings` — прямое название кроя + материала.

**Negative prompts.** OpenAI cookbook **не упоминает** negative prompts вообще. Вместо них Sora использует **Edit Video endpoint** для итеративного refinement. Это важная архитектурная разница с Veo: для Sora мы не передаём «что исключить», а делаем pass2 с правкой.

### 2.3 Kling 3 / Higgsfield

**Особенности.** Kling 3 = image+prompt → video. Главное — image-to-video, текст вторичен. На Higgsfield доступны **20+ pre-optimized motion presets** (cinematic looks). Работа с **start frame + end frame** — две картинки задают состояния, AI интерполирует движение. ([Kling 3 overview](https://higgsfield.ai/blog/Kling-2.6-is-Here-Whats-New), [Kling 2.5 turbo S/E frames](https://higgsfield.ai/blog/A-Guide-to-Kling-Turbo-Start-End-Frame))

**Fashion use-case паттерн.** Outfit drop / product reveal: загружаем 2 фрейма с **той же моделью + позой**, но разной одеждой. Kling делает плавный morph/swap → готовый outfit-transition. Это убирает вопрос character consistency полностью.

**Audio conditioning.** Kling 3 «слышит» аудио и подстраивает движение под бит — даёт film-look без отдельной hand-editing цепочки.

**Promt sweet-spot.** 30–80 слов (короче чем Veo/Sora) + сильный визуальный референс. Текст = модификатор, не основной носитель информации.

### 2.4 Nano Banana / Nano Banana Pro (Gemini 2.5 / Gemini 3 Pro Image)

**Где использовать.** Не как видео-генератор, а как **upstream-step для character consistency**: генерим стабильный «model character» один раз → используем как референс-image для Veo/Sora/Kling. Этот шаг закрывает главный pain — face/hair/body drift между кадрами. ([Nano Banana Pro face consistency guide](https://blog.laozhang.ai/en/posts/nano-banana-pro-face-consistency-guide), [DeepMind Nano Banana Pro](https://deepmind.google/models/gemini-image/pro/))

**Спецификация.**
- Nano Banana 2: до **5 character resemblance + 14 object fidelity** в одной workflow.
- Nano Banana Pro: до **14 reference images, 5 characters tracking**, optimal fidelity при ≤6 references.
- Photoroom уже использует его в «Virtual Fashion Model + Change Fabric Color» pipeline — production proof.

**Промпт-структура.** Edit-style instruction: `Same character as reference image. Change outfit to: black wool turtleneck. Keep face, hair, lighting identical.` Чёткий imperative с ссылкой на reference + что менять / что фиксировать.

### 2.5 Midjourney v7 video

**Профиль.** Сильная stylization (illustration, painterly, editorial), слабый realism для одежды против Veo/Sora. Sweet-spot для нас — **stylized cover frames** (thumbnail / opening shot), не основной motion. Длительность ограничена ~5s в первом релизе. Использовать как «aesthetic seed» — генерим style-reference, скармливаем в Kling 3 как start-frame.

### 2.6 Сводная матрица «модель → роль в нашей фабрике»

| Модель | Роль | Длина клипа | Best for | Не подходит для |
|---|---|---|---|---|
| **Veo 3.1** | Основной motion engine для realism + audio | 8s + chain | Realistic model+garment, dialogue, sync audio | Длинные narrative (>16s без cuts) |
| **Sora 2 Pro** | Cinematic editorial reel, multi-scene | 4/8/12s | High-fashion lookbook, storyboard JSON | Когда нужны жёсткие negative-фильтры |
| **Kling 3** | Outfit-swap, product reveal | 5–10s | Frame-to-frame morph, audio-synced motion | Сложные narrative scene без image refs |
| **Nano Banana Pro** | Character consistency upstream | (image) | Stable model identity для всех клипов | Сам motion |
| **Higgsfield (wrapper)** | Motion-presets хаб + UI поверх Kling/Sora | — | Быстрые viral templates | Production fine control |
| **Midjourney v7 video** | Stylized seed frame | ~5s | Illustration / aesthetic cover | Realistic clothing fit |

---

<a id="sec-3-best-prompts"></a>
## 3. Топ-5 best-practice prompts для fashion-видео ≤30s

Все примеры — production-ready, проверены против промпт-гайдов Veo/Sora/Higgsfield. Слова в `{{}}` — параметры шаблона.

### 3.1 Outfit Transition Reel (Veo 3.1, 8s)

```
Camera locked at eye level, medium shot on 50mm lens.
Subject: {{model_age}}-year-old {{model_ethnicity}} woman, {{model_height}}cm, shoulder-length wavy black hair, natural makeup, wearing {{outfit_a}}.
Action: she takes two confident steps toward camera, performs a 180° turn at second 4, and lands in a relaxed pose with hands on hips. At the moment of the turn, a quick flash transition reveals her now wearing {{outfit_b}}.
Setting: minimalist concrete gallery, polished floor, neutral grey backdrop.
Light: soft three-point studio lighting, key light from camera-left at 45°, fill from camera-right, rim light from behind. Visible fabric weave on garments, fine skin pores.
Style: high-fashion editorial, natural color grade, gentle film grain.
Audio: heels clicking on concrete, subtle fabric rustle, ambient gallery silence.
Negative: distorted hands, extra limbs, motion blur, text overlays, watermark.
```

**Источник техники.** Invideo Veo 3.1 guide + DreamHost Veo 3.1 + Skywork mistakes guide.

### 3.2 Cinematic Editorial Reel (Sora 2 Pro, 12s, JSON storyboard)

```yaml
model: sora-2-pro
size: 1024x1792
seconds: "12"
storyboard: true
style: "Editorial Fashion Reel — glossy modern, editorial-meets-film"
prompt: |
  Cinematic fashion editorial. Full-frame cinema camera intercut with handheld BTS camera for realism.
  Color anchors: matte black, metallic gold, ivory, deep shadow blue, neutral gray. Alternate warm and cool tones each cut.
  Lighting: strobe bursts + constant LED fill + tungsten side spill for warmth.
scenes:
  - timestamp: "0-2s"
    description: "Wide 35mm dolly-in on model in {{outfit_a}}, neutral concrete studio."
    action: "Model takes three steps, turns sharply at beat 2."
  - timestamp: "2-4s"
    description: "85mm macro close-up — fabric texture of {{garment_material}}, gold earring catches strobe."
  - timestamp: "4-6s"
    description: "Whip-pan to model leaning on metal stool, jacket opens during pose."
  - timestamp: "6-8s"
    description: "Circular arc 35mm, fabric lifts from off-camera fan."
  - timestamp: "8-10s"
    description: "Vertical rise crane, model holds stillness, eye contact with camera."
  - timestamp: "10-12s"
    description: "Cut to product macro: {{product_sku}} on ivory pedestal, soft top-down spotlight."
audio:
  - "fashion editorial soundtrack, deep kick drum on each strobe"
  - "fabric rustle, heel taps, distant studio hum"
```

**Источник.** OpenAI Sora 2 cookbook + Higgsfield Sora 2 fashion guide + JSON Prompt Gen.

### 3.3 Outfit Swap (Kling 3, image-to-video, 5s)

**Inputs:**
- `start_frame`: модель в {{outfit_a}}, поза A, neutral background (генерим Nano Banana Pro для consistency)
- `end_frame`: та же модель в {{outfit_b}}, поза A, тот же background

**Prompt:**
```
Smooth outfit morph transition. Same model holds same pose throughout.
Fabric ripples and swaps materially: {{material_a}} dissolves into {{material_b}}.
Camera locks on subject, slow 5% push-in over 5 seconds.
Audio: soft whoosh on transition midpoint, ambient pad.
```

**Почему работает.** Start/End frame убирают character drift полностью. Kling 3 интерполирует движение между двумя фиксированными точками. ([Kling S/E frames guide](https://higgsfield.ai/blog/A-Guide-to-Kling-Turbo-Start-End-Frame))

### 3.4 Product Macro + Hands (Veo 3.1, 6s)

```
Camera: extreme macro 100mm lens, shallow depth of field, locked tripod (that's where the camera is).
Subject: hands of {{model_ethnicity}} woman with neutral manicure unfolding {{product_sku}}, {{garment_material}} fabric.
Action: hands enter frame from left, slowly unfold the garment over 4 seconds, then lift it toward soft window light at second 5.
Setting: ivory linen surface, scattered eucalyptus branches.
Light: large soft key from camera-right window, gentle fill, no harsh shadows.
Style: editorial product macro, natural color, visible fabric weave, soft skin pores.
Audio: paper-soft fabric rustle, distant birdsong, no music.
Negative: extra fingers, distorted hands, harsh shadows, text overlays, logos in background.
```

**Почему важно.** Product macro = главная конверсия для marketplace. Hands — самый частый AI fail (см. секцию 4), поэтому формулировка с `that's where the camera is`, явный счёт по секундам, и негативы по пальцам.

### 3.5 UGC-Style Hook + Outfit (Veo 3.1, 8s, vertical 9:16)

```
9:16 vertical, handheld phone-camera aesthetic, slight stabilization wobble.
Subject: {{model_age}} woman in front-facing selfie position, holding phone at arm's length, wearing {{outfit_a}}.
Action: she turns her head left-right showing earrings (1s), gestures at her outfit with free hand (2-4s), spins once in place (5-6s), lands in final pose looking at camera (7-8s).
Setting: bright daylight bedroom, soft neutral wall, mirror visible behind.
Light: window daylight from camera-left, natural skin tones, no studio polish.
Style: TikTok UGC aesthetic, slight phone-camera color grade, fine skin pores visible.
Audio: ambient room tone, soft fabric movement during spin.
Text overlay: render the words "{{hook_text}}" in white sans-serif at top of frame, frames 0-3s only. (no subtitles)
Negative: studio lighting, professional polish, distorted hands, motion blur, watermark.
```

**Источник техники.** snubroot Veo-3 guide (text on screen workaround) + Arcads UGC patterns + DreamHost Veo 3.1.

---

<a id="sec-4-pitfalls"></a>
## 4. Подвохи AI-генерации одежды — топ-5 fail-зон

### 4.1 Hands & fingers

**Проблема.** Three hands for a split second, distorted fingers — самый частый Veo/Sora fail. is4.ai: success rate первой генерации 40–60%, для fashion с руками — ниже 50%. ([is4.ai 2026 limits](https://is4.ai/blog/our-blog-1/ai-video-generation-limitations-traditional-methods-2026-357), [arsturn Veo 3 character consistency](https://www.arsturn.com/blog/veo-3-character-consistency-guide))

**Mitigation.**
- Closer framing (медиум-шот вместо широкого).
- Slower motion (одно действие — один camera move).
- Reference image для image-to-video (Nano Banana Pro upstream).
- Negative: `distorted hands, extra fingers, duplicate limbs, morphing`.
- Позитивная пере-формулировка: `consistent anatomy, natural hand pose, five fingers`.
- Вообще убирать руки из кадра когда возможно (off-frame action).

### 4.2 Eye direction & gaze

**Проблема.** Модель не смотрит в камеру когда нужно, или взгляд «гуляет» в течение клипа.

**Mitigation.**
- Явный gaze cue: `direct eye contact with camera throughout`, `looks at camera at second 3`.
- `(that's where the camera is)` после описания шота (snubroot Veo).
- Use 50mm+ lens close-up — широкие линзы дают больше шансов сбоя взгляда.

### 4.3 Text, logos, branding

**Проблема.** Veo/Sora плохо рисуют текст: misspellings, garbled subtitles, придуманные логотипы. ([Visla Veo guide](https://www.visla.us/blog/guides/how-to-prompt-veo-3-and-veo-3-1/))

**Mitigation.**
- Никогда не доверять AI отрисовку логотипа — только overlay в пост-продакшене (FFmpeg + PNG).
- Для текста на экране: `(no subtitles)` + добавлять текст в пост-продакшене.
- Если AI должен сказать что-то — формат `{character} says: text` (через двоеточие, не в кавычках).
- Negative: `text overlays, watermark, on-screen captions, brand logos`.

### 4.4 Fabric physics & drape

**Проблема.** Сложные слои, прозрачные ткани, шёлк, шифон — ломаются. Style3D AI: «complex layered outfits or highly transparent fabrics might have slight variations». Чейнмейл и mesh — почти всегда fail. ([Style3D fabric simulation](https://www.style3d.ai/blog/what-is-cloth-simulation-and-how-does-it-transform-digital-fashion/))

**Mitigation.**
- Называть материал явно (`crepe satin`, `merino wool`, `100% cotton denim`), а не general `dress`.
- Описывать поведение: `fabric lifts from fan`, `draped folds at waist`, `subtle ripple in motion`.
- Избегать первой генерации с прозрачными тканями — сначала тест на 1 SKU.
- Для сложных layered looks — Kling 3 с image-to-video (старт от reference photo).

### 4.5 Color accuracy & brand color fidelity

**Проблема.** AI «дрейфует» по цвету: бренд-красный становится оранжево-красным, навигационные цвета меняются между клипами.

**Mitigation.**
- Color anchors в prompt: `garment color: pantone 18-1763 (raspberry red), do not shift hue`.
- Reference image-to-video через Nano Banana Pro с заданным цветом.
- Pre-flight color check после генерации: автоматический pixel-pick на одежде, сравнение с эталоном (CIEDE2000 ΔE < 5).
- Для критичной точности — скриншот после генерации в пост-продакшене переколоризовать (ffmpeg+lut).

### 4.6 Дополнительный fail: face drift между клипами (если делаем reel из 3+ Veo-клипов)

**Mitigation.** Nano Banana Pro upstream → один canonical face → reference image для каждого Veo/Sora-клипа. Aspect ratio референса должен быть **16:9** для Veo (arsturn note).

---

<a id="sec-5-yaml-schema"></a>
## 5. Параметрическая модель шаблона «Фабрики Контента» — YAML

### 5.1 Принципы дизайна

1. **Слой 1 — Library (общий).** Переиспользуемые компоненты: `model_presets`, `location_presets`, `music_presets`, `camera_presets`, `lighting_presets`. Хранятся отдельными YAML-файлами в репозитории `templates/library/`.
2. **Слой 2 — Template (категория).** Шаблон ссылается на presets по `id`, описывает scene-by-scene структуру. `template_id` фиксирован, версионируется.
3. **Слой 3 — Filling (клиент).** Клиент заполняет `{{params}}` своим товаром, цветом, ценой, hook-текстом. Это thin-layer над template.
4. **Render target.** YAML транслируется prompt-builder'ом в нативный формат каждой модели (Veo prompt string / Sora 2 JSON / Kling image+prompt).
5. **Идемпотентность (Закон 11).** Каждое поле имеет default, нельзя получить broken render из неполного filling.

### 5.2 Полная YAML-схема шаблона

```yaml
# templates/outfit-transition-v1.yaml
template_id: "outfit-transition-v1"
version: "1.2.0"
created_at: 2026-05-04T15:00:00Z
category: "fashion.outfit-swap"
title: "Outfit Transition Reel"
description: "Модель с двумя аутфитами, smooth swap на повороте, 12 секунд"
duration_sec: 12
target_platforms: ["instagram_reels", "tiktok", "youtube_shorts"]
aspect_ratios: ["9:16"]   # primary; auto-recrop в 1:1, 16:9 опциональны

# ----- Жёсткие требования к рендеру -----
render:
  primary_engine: "veo-3.1"
  fallback_engines: ["sora-2-pro", "kling-2.6"]
  upstream_consistency: "nano-banana-pro"   # для face/character lock
  expected_iterations: 2-4
  cost_target_usd: 4.50          # верхний предел на 1 видео; см. §7
  generation_timeout_min: 15

# ----- Слой 1: ссылки на library presets -----
references:
  model_preset: "{{model_param}}"        # filling из библиотеки model_presets
  location_preset: "studio-concrete-grey"
  music_preset: "editorial-deep-kick-120bpm"

# ----- Слой 2: scene structure -----
scenes:
  - id: 1
    timestamp: "0-4s"
    type: "establishing"
    visual:
      camera: { lens: "35mm", movement: "dolly-in", framing: "medium-wide" }
      lighting: "soft-3point-studio"
      action: "Model walks 3 steps toward camera in {{outfit_a}}, ends in neutral pose"
      garment: { ref: "{{outfit_a}}", material_emphasis: true }
    transition_out: "match-cut on 180° turn"

  - id: 2
    timestamp: "4-5s"
    type: "transition"
    visual:
      camera: { lens: "35mm", movement: "whip-pan" }
      lighting: "strobe-burst-1frame"
      action: "outfit_swap: {{outfit_a}} → {{outfit_b}}"
      transition_style: "flash-cut"

  - id: 3
    timestamp: "5-9s"
    type: "showcase"
    visual:
      camera: { lens: "85mm", movement: "circular-arc-90deg" }
      lighting: "soft-3point-studio"
      action: "Model rotates pose, garment in {{outfit_b}} catches light, fabric ripples"
      garment: { ref: "{{outfit_b}}", material_emphasis: true }

  - id: 4
    timestamp: "9-12s"
    type: "product_macro"
    visual:
      camera: { lens: "100mm-macro", movement: "static" }
      lighting: "top-down-spotlight-soft"
      action: "Pull focus to {{product_sku}} detail; logo visible at second 11"
    text_overlay:
      enabled: true
      content: "{{cta_text}}"
      style: "white sans-serif, lower-third, 11-12s only"
      method: "post-render-ffmpeg"   # никогда не trust AI с текстом
    music_beat: "drop"

# ----- Слой 3: пользовательские параметры (filling) -----
params:
  required:
    - name: "outfit_a"
      type: "garment_descriptor"
      example: "cream wool turtleneck, high-waisted black trousers"
      validators: ["max_words: 12", "must_include_material"]
    - name: "outfit_b"
      type: "garment_descriptor"
      example: "asymmetric black leather jacket, pleated satin midi skirt"
      validators: ["max_words: 12", "must_include_material"]
    - name: "product_sku"
      type: "string"
      example: "SKU-21345"
    - name: "cta_text"
      type: "string"
      validators: ["max_chars: 30", "no_emoji"]
  optional:
    - name: "model_param"
      type: "ref(model_presets)"
      default: "library:default-fashion-model-v3"
    - name: "color_anchor"
      type: "hex_color"
      default: "#1a1a1a"

# ----- Negative prompts (для Veo) -----
negative:
  global:
    - "distorted hands"
    - "extra fingers"
    - "duplicate limbs"
    - "morphing"
    - "motion blur"
    - "text overlays"
    - "watermark"
    - "garbled brand logos"
  per_scene:
    scene_4:
      - "warped product details"

# ----- QC чек-листы (автоматические) -----
quality_gates:
  - name: "color_fidelity"
    check: "ciede2000_delta_e_lt: 5"
    target: "{{color_anchor}}"
    samples: 10
  - name: "face_consistency"
    check: "face_embedding_cosine_gt: 0.85"
    reference: "model_preset.canonical_face"
  - name: "duration_match"
    check: "abs(duration - 12.0) < 0.3"
  - name: "no_text_artifacts"
    check: "ocr_detected_words: []"   # никаких unintended captions
```

### 5.3 Library-файл `model_presets/default-fashion-model-v3.yaml`

```yaml
preset_id: "default-fashion-model-v3"
version: "3.0.0"
canonical_face_image: "library/faces/model-v3-canonical-16x9.png"   # для Nano Banana Pro
canonical_face_embedding: "library/faces/model-v3-canonical.embedding"

attributes:
  age_range: [22, 28]
  ethnicity: "racially-ambiguous"
  height_cm: 175
  build: "slim-athletic"
  hair: "shoulder-length wavy black"
  skin_tone: "neutral-medium"
  makeup: "natural editorial"

prompt_fragment: |
  Subject: 24-year-old racially-ambiguous woman, 175cm, slim-athletic build,
  shoulder-length wavy black hair, neutral editorial makeup, fine skin pores visible.

stability_notes:
  - "Always pass canonical_face_image as reference for image-to-video"
  - "Reference image MUST be 16:9 aspect ratio for Veo 3.1"
  - "Do not vary attributes between clips of the same template run"
```

### 5.4 Library-файл `location_presets/studio-concrete-grey.yaml`

```yaml
preset_id: "studio-concrete-grey"
prompt_fragment: |
  Setting: minimalist concrete gallery, polished grey floor with subtle reflections,
  neutral grey backdrop, no furniture, no logos visible.
lighting_default: "soft-3point-studio"
ambient_audio: "distant gallery hum, no music bed"
```

### 5.5 Batch-генерация N вариантов

```yaml
# campaigns/spring-2026-batch-001.yaml
campaign_id: "spring-2026-batch-001"
template_id: "outfit-transition-v1"
template_version: "1.2.0"
client_id: "brand-xyz"
variants:
  - sku: "SKU-21345"
    outfit_a: "cream wool turtleneck, high-waisted black trousers"
    outfit_b: "asymmetric black leather jacket, pleated satin midi skirt"
    cta_text: "Shop Spring '26"
    color_anchor: "#1a1a1a"
  - sku: "SKU-21346"
    outfit_a: "..."
    outfit_b: "..."
    cta_text: "Limited Drop"
  # ... до 50 вариантов в одном batch
limits:
  max_parallel_generations: 5
  total_cost_cap_usd: 250.00
```

---

<a id="sec-6-prompt-builder"></a>
## 6. Prompt-Builder: YAML → text для Veo / Sora / Kling

### 6.1 Архитектура

```
campaign.yaml + template.yaml + library/*.yaml
        ↓
[Resolver] разрешает ссылки {{param}}, ref(library:...)
        ↓
[Renderer per engine]
  ├─ veo-3.1   → multi-line text prompt + negative list
  ├─ sora-2    → JSON storyboard
  └─ kling-2.6 → start_frame + end_frame + short prompt
        ↓
[API client] (Vertex AI / OpenAI / fal.ai)
        ↓
[Post-processor] text overlay (FFmpeg), color correction, trim, multi-aspect-recrop
        ↓
[QC gates] face embedding, OCR, color fidelity
        ↓
final video → S3 + manifest.json
```

### 6.2 Render для Veo 3.1 (псевдокод)

```python
def render_veo(template, filling, library) -> VeoRequest:
    model = library.resolve(filling["model_param"])
    location = library.resolve(template["references"]["location_preset"])

    scenes_text = []
    for scene in template["scenes"]:
        scenes_text.append(render_scene_veo(scene, filling, model, location))

    prompt = f"""
{model.prompt_fragment}
{location.prompt_fragment}

Sequence ({template['duration_sec']}s total):
{chr(10).join(scenes_text)}

Style: high-fashion editorial, natural color grade, fine skin pores, visible fabric weave.
Audio: {library.music(template).description}
"""
    negative = ", ".join(template["negative"]["global"])
    return VeoRequest(
        prompt=prompt.strip(),
        negative_prompt=negative,
        reference_image=model.canonical_face_image_16x9,
        aspect_ratio="9:16",
        duration_seconds=8,   # Veo max; 12s = chain через Extend
    )
```

### 6.3 Render для Sora 2 (JSON storyboard напрямую)

YAML → JSON 1-в-1 mapping (см. §3.2 пример). Главное — `storyboard: true` + per-scene `description/camera/lighting/transition` + global `style`.

### 6.4 Render для Kling 3 (image-to-video)

1. Сгенерить **start_frame** через Nano Banana Pro: `{model.canonical_face_image_16x9}` + `outfit_a` (image edit).
2. Сгенерить **end_frame**: тот же model, та же поза, **outfit_b**.
3. Короткий prompt (30–80 слов) описывающий transition.
4. Submit через fal.ai / Replicate.

---

<a id="sec-7-benchmarks"></a>
## 7. Cost & throughput benchmarks

### 7.1 Cost per second (API, май 2026)

| Модель | Resolution | $/sec | 8s clip cost | 12s clip cost | Источник |
|---|---|---|---|---|---|
| Veo 3.1 Lite | — | $0.05 | $0.40 | $0.60 | aifreeapi |
| Veo 3.1 Fast | 720p | $0.10–0.15 | $0.80–1.20 | $1.20–1.80 | costgoat |
| Veo 3.1 Standard | 720p+audio | $0.40 | $3.20 | $4.80 | costgoat |
| Veo 2 | — | $0.35–0.50 | $2.80–4.00 | — | costgoat |
| Sora 2 | 720p | $0.10 | $0.80 | $1.20 | aifreeapi |
| Sora 2 Pro | 720p | $0.30 | $2.40 | $3.60 | aifreeapi |
| Sora 2 Pro | 1024p (1792×1024) | $0.50 | $4.00 | $6.00 | aifreeapi |
| Kling 3 (Higgsfield/fal.ai) | — | ~$0.08–0.12 | $0.64–0.96 | ~$1.20 | Higgsfield Kling 3 |
| Runway Gen-3 | — | ~$0.05 / sec equivalent | — | — | Atlas Cloud |

### 7.2 Подписки vs API (для нашей нагрузки 1550 видео/мес)

- **Google AI Ultra $249.99/mo** = ~250 Veo 3.1 видео/мес → не хватает. На 1550 нужен **~$2500/мес pure API** (Veo 3.1 Standard 8s) или **~$925/мес** на Veo 3.1 Fast 8s + post.
- **ChatGPT Pro $200/mo** = 10K credits → достаточно для тестов, но коммерчески нужен API.
- **Sora 2 Pro 12s @ 1024p** = $6/видео → $9300/мес для 1550 видео — слишком дорого как primary.

### 7.3 Расчёт на наш сценарий (один шаблон outfit-transition, 12s, 1550 видео/мес)

| Сценарий | Engine mix | $/видео | Месячный API | Iterations включены |
|---|---|---|---|---|
| **Бюджетный** | Veo 3.1 Fast 8s + Kling 3 5s outfit-swap + post-stitch | ~$2.00 | $3 100 | 2x retries average |
| **Средний (рекомендуемый)** | Veo 3.1 Standard 8s + Nano Banana upstream + post overlay | ~$4.50 | $6 975 | 3x retries |
| **Премиум** | Sora 2 Pro 1024p 12s storyboard | ~$8.50 | $13 175 | 2x retries |

**Cost target в шаблоне** (`render.cost_target_usd: 4.50`) = средний сценарий, который укладывается в бюджет 7K USD/мес на API при 1550 роликах.

### 7.4 Generation time

- Veo 3.1: 8s clip ≈ **2–5 минут** генерации, Pro mode медленнее (Atlas Cloud).
- Sora 2: standard 8s ≈ **1–3 минуты**, Pro 12s ≈ **3–7 минут**.
- Kling 3: 5s clip ≈ **1–2 минуты** (image-to-video быстрее text-to-video).
- На 1550 видео/мес при 3 параллельных генерациях получаем ~50 часов чистого generation time = ~2 машиночаса в сутки. Узкое место не время, а deduplication/QC.

### 7.5 Iterations and success rate

is4.ai ([2026 limits report](https://is4.ai/blog/our-blog-1/ai-video-generation-limitations-traditional-methods-2026-357)):

- Первая генерация совпадает с ожиданиями: **40–60%**.
- Среднее число попыток до usable output: **3–8**.
- Социальный/short-form контент (Reels/TikTok): success **50–65%** на итерацию.
- Стилистический/экспериментальный: **60–75%**.
- Visual-style-test: **70–80%**.

> Закладываем в pipeline `expected_iterations: 2-4`, hard cap 6, gracefully fail с manual-review-flag после.

---

<a id="sec-8-sources"></a>
## 8. Источники

### Конкуренты-сервисы

- [Botika products](https://botika.com/products) / [Botika pricing](https://botika.com/pricing) / [Botika new](https://botika.com/resources/thethread/meet-the-new-botika-ai-for-fashion-teams-driving-quality-at-scale)
- [Veesual](https://www.veesual.ai/) / [Switch Model](https://www.veesual.ai/solutions/switch-model) / [VITON-GAN explainer](https://www.veesual.ai/blog/virtual-try-on-styling-fitting)
- [Resleeve studio](https://resleeve.ai/) / [AI Clothes Changer](https://resleeve.ai/blog/ai-clothes-changer-the-game-changing-virtual-outfit-experience)
- [Flair.ai fashion models](https://flair.ai/product-category-features/fashion-models) / [Flair main](https://flair.ai/)
- [Arcads](https://www.arcads.ai/) / [Arcads review codeitbro](https://www.codeitbro.com/blog/arcads-ai-review)
- [Creatify product templates](https://creatify.ai/blog/product-to-video-templates-pre-built-ad-formats-for-instant-campaigns) / [Creatify main](https://creatify.ai/)
- [Topview product video](https://www.topview.ai/ai-product-video) / [Topview Video Agent](https://www.topview.ai/guides/video-agent) / [WithLore deep-dive](https://www.withlore.co/blog/topview-ai-viral-video-agent/)
- [AdCreative.ai](https://www.adcreative.ai/) / [Pencil AI](https://trypencil.com/)
- [Pictory / Invideo coverage AdStellar](https://www.adstellar.ai/blog/best-ugc-ad-creation-platforms)

### Promt-инжиниринг — официальные docs

- **Sora 2 (OpenAI)**: [Cookbook prompting guide](https://developers.openai.com/cookbook/examples/sora/sora2_prompting_guide) / [Sora 2 API video generation guide](https://developers.openai.com/api/docs/guides/video-generation)
- **Veo on Vertex AI (Google)**: [Vertex AI prompt guide](https://docs.cloud.google.com/vertex-ai/generative-ai/docs/video/video-gen-prompt-guide)
- **Nano Banana Pro (DeepMind)**: [Gemini 3 Pro Image](https://deepmind.google/models/gemini-image/pro/) / [Google blog Nano Banana 2](https://blog.google/innovation-and-ai/technology/ai/nano-banana-2/) / [Nano Banana Pro on GCP](https://cloud.google.com/blog/products/ai-machine-learning/nano-banana-pro-available-for-enterprise)
- **Kling 3 (Higgsfield)**: [Kling 3 overview](https://higgsfield.ai/blog/Kling-2.6-is-Here-Whats-New) / [Kling 3 audio prompts](https://higgsfield.ai/blog/kling-2.6-audio-prompt-examples) / [Kling Start/End frames](https://higgsfield.ai/blog/A-Guide-to-Kling-Turbo-Start-End-Frame) / [Kling motion control](https://higgsfield.ai/blog/Kling-2.6-Motion-Control-Full-Guide)

### Promt-инжиниринг — комьюнити-гайды

- Veo 3.1: [Invideo guide](https://invideo.io/blog/google-veo-prompt-guide/) / [DreamHost](https://www.dreamhost.com/blog/veo-3-1-prompt-guide/) / [LTX Studio](https://ltx.studio/blog/veo-prompt-guide) / [story321 playbook](https://story321.com/blog/veo-31-prompt) / [imagine.art](https://www.imagine.art/blogs/veo-3-1-prompt-guide) / [VEED](https://www.veed.io/learn/veo-3-1-prompts) / [Skywork mistakes](https://skywork.ai/blog/veo-3-1-prompting-mistakes-fixes/) / [Skywork errors](https://skywork.ai/blog/llm/veo-3-1-troubleshooting-common-errors-2/) / [snubroot Veo-3-Prompting-Guide](https://github.com/snubroot/Veo-3-Prompting-Guide) / [Visla](https://www.visla.us/blog/guides/how-to-prompt-veo-3-and-veo-3-1/) / [Replicate blog](https://replicate.com/blog/using-and-prompting-veo-3) / [Leonardo](https://leonardo.ai/news/mastering-prompts-for-veo-3/) / [Sider field guide](https://sider.ai/blog/ai-tools/best-prompt-techniques-for-veo-3_1-video-output-a-field-guide-to-cinematic-control) / [Arsturn troubleshooting](https://www.arsturn.com/blog/troubleshooting-common-veo-3-problems-a-users-guide) / [Arsturn character consistency](https://www.arsturn.com/blog/veo-3-character-consistency-guide) / [SeaArt 22 prompts](https://www.seaart.ai/blog/veo-3-1-prompts) / [GeeLark](https://www.geelark.com/blog/how-to-write-veo-3-1-prompts/) / [UlazAI examples](https://ulazai.com/veo3-prompt-examples/)
- Sora 2: [Higgsfield Sora 2 guide](https://higgsfield.ai/sora-2-prompt-guide) / [Atlabs](https://www.atlabs.ai/blog/sora-2-prompt-guide) / [WaveSpeedAI](https://wavespeed.ai/blog/posts/sora-2-prompting-tips-better-videos-2026/) / [aisuperhub](https://www.aisuperhub.io/blog/sora-2-prompt-templates-and-examples) / [theaivideocreator cheatsheet](https://www.theaivideocreator.ai/p/sora-2-prompting-cheatsheet) / [JSON Prompt Gen](https://solvingtools.github.io/JSON-Prompt-Gen/src/blog/sora-json-prompt-guide/index.html) / [shijincai sora-prompt-generator](https://github.com/shijincai/sora-prompt-generator) / [simalabs JSON templates](https://www.simalabs.ai/resources/top-50-sora-2-prompt-examples-4k-cinematic-drone-footage-json-templates) / [metricsmule prompts](https://metricsmule.com/ai/sora-2-prompts/) / [digen guide](https://resource.digen.ai/openai-sora-text-to-video-guide/)
- Nano Banana: [aifacefy consistent characters](https://aifacefy.com/blog/detail/How-to-Generate-Consistent-Characters-with-Nano-Banana-Gemini-2-5-Flash-f04e03416688/) / [LaoZhang face consistency](https://blog.laozhang.ai/en/posts/nano-banana-pro-face-consistency-guide) / [media.io character consistency](https://www.media.io/image-effects/character-consistency-nano-banana.html) / [getimg explainer](https://getimg.ai/blog/what-is-nano-banana-google-gemini-2-5-flash-image-explained)

### Pricing & throughput

- [aifreeapi Sora 2 pricing](https://www.aifreeapi.com/en/posts/sora-2-api-pricing-quotas) / [aifreeapi Veo 3.1 pricing](https://www.aifreeapi.com/en/posts/veo-3-1-pricing) / [costgoat Veo](https://costgoat.com/pricing/google-veo) / [costgoat Sora](https://costgoat.com/pricing/sora) / [Atlas Cloud cheapest APIs 2026](https://www.atlascloud.ai/blog/guides/cheapest-ai-video-generation-api-2026) / [veo3ai pricing](https://www.veo3ai.io/blog/veo-3-pricing-2026) / [JuheAPI Veo vs Sora](https://www.juheapi.com/blog/veo-3-1-api-pricing-vs-sora-2-runway-gen-3-comparison-wisdom-gate) / [Apiyi Veo 3.1 Lite](https://help.apiyi.com/en/google-veo-3-1-lite-api-video-generation-cost-effective-guide-en.html) / [glbgpt Veo subscription](https://www.glbgpt.com/hub/how-much-is-veo-3-1-subscription-cost/) / [Atlas Cloud best models 2026](https://www.atlascloud.ai/blog/guides/best-ai-video-generation-models-2026)

### Limits, fabric physics, troubleshooting

- [is4.ai 2026 limits](https://is4.ai/blog/our-blog-1/ai-video-generation-limitations-traditional-methods-2026-357) / [is4.ai state of AI video 2026](https://is4.ai/blog/our-blog-1/ai-video-generation-2026-what-works-what-doesnt-340) / [digen benchmarks 2026](https://resource.digen.ai/text-to-video-ai-benchmarks-2026/) / [Style3D AI fabric simulation](https://www.style3d.ai/blog/what-is-cloth-simulation-and-how-does-it-transform-digital-fashion/) / [Style3D fabric textures tools](https://www.style3d.ai/blog/what-is-the-best-ai-tool-for-creating-realistic-fabric-textures-in-fashion-design/) / [Style3D fashion videos 2026](https://www.style3d.ai/blog/how-to-create-ai-fashion-videos-from-static-images-in-2026/) / [ReelMind cloth simulation](https://reelmind.ai/blog/ai-video-cloth-and-fabric-simulation-realistic-movement) / [arxiv D-Garment](https://arxiv.org/html/2504.03468v1) / [Wearview top 7 AI fashion video tools](https://www.wearview.co/blog/best-ai-fashion-video-generators) / [Pinggy best models 2026](https://pinggy.io/blog/best_video_generation_ai_models/) / [Higgsfield 2026 predictions](https://higgsfield.ai/blog/top-5-predictions-for-ai-video-generation-in-2026) / [Google forum negative prompts issue](https://discuss.google.dev/t/veo-3-1-model-ignores-negative-prompts-for-non-humanoid-character-anatomy/351836)

### JSON pipelines / production tools

- [json2video API](https://json2video.com/) / [jsontovideo](https://jsontovideo.org/) / [ImagineArt JSON prompting](https://www.imagine.art/blogs/json-prompting-for-ai-video-generation) / [LTX Studio JSON prompting](https://ltx.studio/blog/json-prompting-for-video-image-generation) / [n8n natural→JSON workflow](https://n8n.io/workflows/6832-convert-natural-language-to-video-json-prompts-with-gpt-and-gemini-for-veo-3/) / [editingtools Veo3 prompt generator](https://editingtools.io/veo/) / [Medium structured creativity](https://medium.com/@ai.in.motion.blog/video-generation-with-json-prompting-my-deep-dive-into-structured-creativity-e89b4b82c1b8)

---

# 9. ОБНОВЛЕНИЕ СТЕКА — Sora 2 → Seedance 2.0 (v1.1, 2026-05-04)

> **Причина обновления:** OpenAI API заблокирован в РФ. Биллинг и стабильный доступ к Sora 2 без серого VPN-шлюза невозможны. Sora 2 убрана из основного стека. На её место — **Seedance 2.0 (ByteDance)** как primary partner для Veo 3.1.
>
> Все секции выше (1-8), описывающие Sora 2 — оставлены как **исторический референс** (часть промпт-паттернов Sora 2 переносима на Seedance 2.0 multi-shot framework). Везде, где в production-коде / шаблонах упоминается `sora_2_*` engine — заменить на `seedance_2_0_*` по таблице маппинга в § 9.7.

## 9.1 Новый стек (май 2026)

**ОСНОВНОЙ:**

| # | Модель | Вендор | Доступ из РФ | Роль |
|---|--------|--------|--------------|------|
| 1 | **Veo 3.1** | Google (Vertex AI / Gemini API) | через прокси (биллинг GCP под зарубежной картой) | **Cinematic / Hero / Faces / Audio / 4K** |
| 2 | **Seedance 2.0** | ByteDance (Volcengine Ark / Doubao) | прямой, без VPN | **Motion / Transitions / Multi-shot 15s / Multi-ref до 15** |

**ALT (fallback и комбо):**

| # | Модель | Вендор | Доступ из РФ | Роль |
|---|--------|--------|--------------|------|
| 3 | Kling 3 | Kuaishou (КНР) | прямой | Face-consistency fallback ($0.07-0.14/сек) |
| 4 | Hailuo 2.3 | MiniMax (КНР) | прямой | Бюджетный motion ($0.28-0.49/clip) |
| 5 | Higgsfield AI | US | через прокси | Film-look fallback |
| 6 | Vidu Q3 | Shengshu (КНР) | прямой | Дешёвый I2V Turbo $0.034/сек |
| 7 | Runway Gen-4 | Runway (US) | через прокси | Top-quality fallback |
| 8 | Pika 2.0 | Pika (US) | через прокси | Stylization |
| 9 | Luma Dream Machine | Luma (US) | через прокси | Motion-art fallback |
| 10 | Kandinsky 4.1 Video | Сбер (РФ) | прямой | Резерв (требует коммерческого договора) |
| 11 | **Nano Banana Pro** | Google DeepMind | через прокси | Character anchor / VTO / refs |
| 12 | Midjourney v7 video | Midjourney (US) | через прокси | Mood frames / стилизация |

## 9.2 Veo 3.1 — fashion-prompt best practices (расширение § 2.1)

**Структура (5 компонентов, Google Cloud Blog):**
```
[Cinematography] + [Subject] + [Action] + [Context] + [Style & Ambiance]
```

**Критичное для одежды:**

| Аспект | Как формулировать | Пример |
|--------|-------------------|--------|
| Fabric physics | материал + поведение | "silk dress flowing in breeze, natural drape" |
| Drape / fold | named patterns | "realistic draping and fold patterns" |
| Light reflection | материал-cue даёт light profile | "charcoal canvas, cotton, silk" |
| Fit | terminology | "oversized", "tailored", "body-hugging" |
| Colour accuracy | named colours, не "красивый" | "cream on cream", "muted sage green" |
| Body-anchor | identity ahead of fit | "tall female model with auburn hair" + outfit |

**Subject pinned в первых 20 словах** — иначе Veo дрейфует на multi-shot.

**Спецификации:**
- Max duration: **8 сек** (timestamp prompting позволяет 4 sub-shots внутри одного gen)
- Resolutions: 720p / 1080p (4K через Imagen upscale)
- Aspect: 16:9, 9:16
- Audio: native sync (dialog + SFX + music)
- I2V: до **3 reference images** (Ingredients to Video) для cross-shot consistency
- First & Last Frame: natural transition + audio
- Sweet-spot prompt: 100-150 слов на single shot, 200-400 на timestamp multi-shot

**Negative prompt синтаксис** — описать что НЕ хочешь, без слов "no/don't":

✅ `"clean studio backdrop, no urban background, no harsh shadows on face"`
❌ `"don't add buildings"`

**5 fashion prompts для Veo 3.1:**

**(1) Lookbook hero — 8s 1080p**
```
Medium shot, slow tracking left dolly. A tall female model wearing a flowing
linen suit, cream on cream, walks through a sunlit Mediterranean courtyard.
High-key studio light spilling through arched windows, soft shadows on the
linen, natural drape and fold movement on the trousers. Shot on ARRI Alexa,
shallow depth of field, 35mm lens. Quiet luxury aesthetic, slow-motion,
golden hour ambiance. Ambient noise: distant cicadas, soft breeze rustling
through olive leaves. Music: minimalist piano, single repeating note.
```

**(2) Fabric close-up — 6s 4K**
```
Extreme close-up, macro lens. A charcoal silk scarf falls through still air.
The lightweight fabric floats and billows naturally with realistic air
resistance, settling onto polished marble with natural draping and fold
patterns. Studio overhead softbox, side rim light at 45 degrees catching
the silk weave. Slow-motion 240fps look. Style: tactile editorial,
Hermès-grade material storytelling. No human hands, no harsh shadows, no
plastic look.
```

**(3) Try-on / GRWM с reference — 8s 9:16, I2V**
```
[Reference image: model headshot]
Close-up portrait, soft front-light. The same woman from the reference
adjusts the collar of an oversized charcoal wool coat in a mirror. Subtle
micro-expression: half-smile, eye flicker to camera. Camera holds. GRWM
documentary style, raw morning light through sheer curtains. SFX: fabric
swish on fabric, soft footsteps on hardwood. Maintain facial features fully
consistent with the reference image — same eyes, same jawline, same hair
length and parting.
```

**(4) Runway walk — 8s 9:16**
```
Medium-wide shot, slow tracking right at walking pace. A tall and stylish
female model wearing a beige trench coat with black leather Chelsea boots
walks confidently down a Paris Fashion Week runway. Catwalk lights from
above, hard shadow under the brim of her hat. Audience in soft bokeh
background. Sony Venice, 50mm lens, shallow DoF. Editorial fashion week
documentary. SFX: heels clicking on polished floor, distant camera shutter
clicks, low electronic ambient music.
```

**(5) Product 360 — 8s 1:1**
```
Slow 360-degree orbit around a folded cream cashmere sweater on a polished
walnut surface. Fabric texture visible — soft, brushed, subtle natural
weave. Single overhead softbox, warm 3200K, deep shadow falloff. 85mm lens,
f/2.8. Aesop-grade minimalist product photography, slow-burn luxury.
Ambient noise: faint room tone, no music. No human, no logo, no extra
props.
```

**Цены Veo 3.1 (май 2026):**

| Tier | Vertex AI | fal.ai | Replicate |
|------|-----------|--------|-----------|
| Standard (с audio) | $0.40-0.75/сек | $0.40/сек | $0.50/сек |
| Fast (без audio) | $0.10-0.15/сек | — | — |
| Lite (с audio) | **$0.05/сек** | — | — |

> 8 сек на Lite = $0.40, на Standard = $3.20-6.00.
> Для 1550 видео × 8 сек: Lite ≈ $620/мес, Standard ≈ $5-9k/мес.

## 9.3 Seedance 2.0 — fashion-prompt best practices (НОВОЕ, заменяет § 2.2 Sora 2)

**Базовая структура (Oimi AI guide):**
```
[Style Definition] + [Scene Description] + [Camera Movement] + [Timeline] + [Audio/Music] + [Fine Details]
```

**Multi-shot framework (Atlabs.ai 47-prompt guide):**
```
[Style / Era] · [Duration] · [Scene] · [Characters]
[Shot 1: Visuals + Action + Details]
[Shot 2: Visuals + Action + Details]
[Shot 3: Visuals + Action + Details]
+ [Technical specs: resolution, aspect ratio, seconds]
```

**Lead with style** — Seedance взвешивает style-токены сильнее Veo. Первое слово часто определяет look.

**Язык:** английский — стабильно. Китайский нужен только для локальных Doubao-фич ("国风", "汉服"). **Code-switch рискован.** Для нашей фабрики: всё на английском, RU→EN translate перед отправкой.

**Movement-keywords (где Seedance сильнее Veo):**
- `simulated crane shot rising`
- `tracking shot at walking pace`
- `whip pan transition`
- `match cut between shots`
- `slow-motion fabric flutter`
- `morphing transition: <A> smoothly transforms into <B>`
- `camera handheld documentary style`
- `dolly zoom (vertigo effect)`

**Multi-modal references — главная фишка Seedance 2.0:**
- **9 images** (`@image1 ... @image9`) — characters, products, textures
- **3 video clips** (`@video1 ... @video3`) — стилевые / motion-refs
- **3 audio tracks** (`@audio1 ... @audio3`) — голоса, музыка, ambient

**Character consistency (правило 20-30 слов):**
```
Reference @Image1 for the character's facial features.
Shot 1: The woman wears a business suit, walking confidently...
Shot 2: Same woman now in athletic gear, jogging at sunrise...
Maintain facial features and wardrobe fully consistent with @Image1.
Reiterate: short silver hair, mole under left eye, hazel eyes.
```

⚠️ Закон: model weighs first 20-30 words heavily. Если subject не "pinned" немедленно — multi-shot engine **галлюцинирует новых персонажей** на переходах.

**5 fashion prompts для Seedance 2.0:**

**(1) Material-morph runway — 10s**
```
[Style] Hollywood Haute Couture Fantasy blockbuster, 8K ultra-clear,
photorealistic, high-fashion editorial style.
[Scene] An Asian female model with a highly recognizable, high-fashion face
walks coolly on a mirrored water surface lit from below.
[Action] She wears not fabric, but a long dress made of flowing, real
liquid blue-and-white porcelain that ripples with each step, ceramic
fragments forming and dissolving naturally.
[Camera] Slow tracking shot at hip level, ARRI Alexa, 50mm lens, shallow DoF.
[Audio] Ambient: water drips on stone, faint koto string in background.
1080p, 16:9, 10 seconds.
```

**(2) Outfit-transition — 8s**
```
[Style] Paris Fashion Week documentary, photorealistic.
[Scene] An extremely beautiful model walks the Paris Fashion Week runway.
[Action] Her outfit smoothly transforms from a mini-skirt to a long skirt;
simultaneously, the top transforms into a long dress. Fabric physics
realistic throughout — natural drape, fold, weight.
[Camera] Static medium shot, slight handheld breathing.
[Audio] Crowd murmur, electronic runway track.
720p, 9:16, 8 seconds.
```

**(3) 3-shot product story — 12s, multi-shot**
```
[Style] Aesop-grade minimalist product film, muted palette.
[Characters] Reference @Image1 for the model's appearance.

Shot 1 (0-4s): Close-up of @Image1's hands taking a folded cream cashmere
sweater out of a kraft paper box. Shallow DoF on the fabric weave.
Shot 2 (4-8s): Medium shot of @Image1 holding the sweater up to soft
window light, examining the texture. Whip pan transition.
Shot 3 (8-12s): Wide shot of @Image1 wearing the sweater, walking out
of frame against a warm beige backdrop.

Maintain facial features and wardrobe consistent with @Image1.
[Audio] Ambient: paper rustle, fabric on fabric, soft acoustic guitar.
1080p, 1:1, 12 seconds.
```

**(4) Sneaker rotation ad — 6s**
```
[Style] Nike-grade product reveal, photorealistic.
[Scene] A pair of fresh white-on-white running sneakers rotates on a
black turntable.
[Action] Slow 360-degree rotation, camera orbits in opposite direction
creating dual-rotation effect. Sneaker laces flutter slightly from a
hidden fan.
[Camera] Macro lens at midsole height, dramatic side rim light, hard
shadow falloff to black.
[Audio] Deep sub-bass hit on full reveal, brand sting.
1080p, 1:1, 6 seconds.
```

**(5) Editorial movement (dynamic) — 10s**
```
[Style] Cinematic human documentary, authentic handheld tracking, Sony A7S3.
[Scene] @Character1 (reference @Image1) wearing a flowing red silk dress
walks through a wheat field at golden hour.
[Action] She turns toward camera mid-stride, the silk dress catches the
wind and billows out behind her with realistic fabric physics. Her hair
moves naturally with the wind direction.
[Camera] Handheld tracking shot, 35mm, wide aperture f/1.8, slow-motion
60fps. Sun flares through the silk fabric briefly mid-shot.
[Audio] Wind through wheat, faint cinematic strings rising.
4K, 9:16, 10 seconds.
Maintain facial features fully consistent with @Image1.
```

**Спецификации:**

| Параметр | Значение |
|----------|----------|
| Max duration / generation | **15 секунд** (vs 8 у Veo) |
| Native resolution | до 2K (4K на Higgsfield интеграции) |
| Reference inputs | 9 images + 3 videos + 3 audios = 15 одновременно |
| Audio | native sync, всегда включено и оплачивается |
| **Standard 720p** | **$0.30/сек** → $3.03 за 10 сек |
| **Fast 720p** | **$0.24/сек** → $2.42 за 10 сек |
| Доступ | Volcengine Ark API + Doubao + fal.ai + Higgsfield + Replicate |
| **РФ** | **прямой через Volcengine, без VPN** |

## 9.4 Combo-стратегия Veo 3.1 + Seedance 2.0 (заменяет § 2.6 матрицу)

**Когда какой движок:**

| Сцена / задача | Лучший движок | Почему |
|----------------|---------------|--------|
| Cinematic hero, brand-look, 4K | **Veo 3.1 Standard** | character/product lock, 4K, native audio |
| Faces close-up, micro-expressions, dialog | **Veo 3.1** | "faces handled with particular care, reducing uncanny valley" |
| Dynamic motion, dance, runway, transitions | **Seedance 2.0** | physically-grounded camera, no motion smearing |
| Multi-shot story 10-15 сек | **Seedance 2.0** | 15 сек native, multi-ref до 15 inputs |
| Tactile fabric close-up | **Veo 3.1** (4K + audio) ИЛИ **Seedance 2.0** (motion) | разная стилистика |
| Try-on / virtual fitting | **Veo 3.1 I2V** + Nano Banana refs | reference-image locking |
| Product 360 rotation | **Seedance 2.0** | physical camera simulation |
| Бюджетный draft / A/B-test | **Veo 3.1 Lite** ($0.05/сек) | в 6× дешевле Standard |
| Fallback motion | **Vidu Q3 Turbo** ($0.034/сек) | для thumbnail-видео |

**30-секундный fashion-ролик (TikTok/Reels) — рекомендуемая структура:**

```
0-5s   HOOK       — Veo 3.1 (face close-up, hook-line dialog)         — 1080p
5-15s  DEMO       — Seedance 2.0 (3-shot product story, whip-pan cuts) — 1080p
15-25s TRANSFORM  — Seedance 2.0 (outfit-morph / motion)               — 1080p
25-30s CTA        — Veo 3.1 (hero shot 4K via Imagen upscale + brand)  — 1080p→4K
```

**Стоимость 1 ролика 30 сек:**

| Tier | Veo (10s) | Seedance (20s) | Итого / ролик |
|------|-----------|----------------|---------------|
| Standard combo | $4-7.50 | $6.00 | **$10-13.50** |
| Lite combo | $0.50 | $4.80 | **$5.30** |

**При 1550 ролик/мес:**
- Standard combo: **$15-21k/мес**
- Lite combo: **~$8k/мес**

## 9.5 Workflow: Nano Banana → Veo / Seedance (заменяет § 6.1 архитектуру)

```
STEP 1 — CHARACTER ANCHOR (Nano Banana Pro / Gemini 3 Pro Image)
  Input:  client photo + outfit photos
  Output: 5-10 reference images (front/side/back/detail/face)
  Cost:   ~$0.05/image × 10 = $0.50 per character

STEP 2 — TRY-ON / WARDROBE (Nano Banana Pro VTO)
  Input:  character image + clothing image
  Output: model-wearing-clothing image (multi-view)
  Cost:   ~$0.10/image × 5 views = $0.50

STEP 3 — UPSCALE (Imagen 4 для hero-frames)
  Input:  3-4 hero frames
  Output: 4K refined frames
  Cost:   ~$0.04/image × 4 = $0.16

STEP 4 — VIDEO GENERATION
  Veo 3.1:    cinematic / face / hero (5-10 сек)
  Seedance:   motion / transitions / multi-shot (10-20 сек)
  Cost:       $5-13 за 30-сек ролик (см. 9.4)

STEP 5 — POST (FFmpeg pipeline)
  - cut между Veo и Seedance кусками
  - audio mix (если выключали native в Veo)
  - watermark / brand overlay
  - 9:16 master + 1:1 + 16:9 derivatives
```

**Fully-loaded cost ролика:**
- Anchor: $0.50 + VTO: $0.50 + Upscale: $0.16
- Video: $5.30 (Lite) или $13.50 (Standard)
- LLM (prompt-builder): ~$0.05
- **Total: $6.50 (Lite) — $14.70 (Standard) на 1 ролик**

## 9.6 Quality gate (расширение § 5.2 QC чек-листов)

После генерации каждого шота:

| Проверка | Инструмент | Threshold | Action на fail |
|----------|-----------|-----------|----------------|
| Face consistency vs anchor | ArcFace embedding distance | < 0.65 | re-gen с усиленным `consistency_anchor` |
| Hand anomaly (extra fingers) | YOLO hand-detector + finger count | exactly 4+thumb | crop / re-gen |
| Fabric distortion | Optical flow anomaly score | < 0.30 | swap engine: Veo→Seedance или наоборот |
| Text-in-frame | PaddleOCR | empty (text adds in post) | inpaint / re-gen |
| Uncanny "glow" | LPIPS perceptual diff vs ref | < 0.45 | reduce style intensity |
| Audio sync | librosa onset vs visual cuts | drift < 100ms | re-mux в FFmpeg |

При 2 fail подряд на одном шоте — fallback на Kling 3 / Hailuo (motion) или Higgsfield (cinematic).

## 9.7 Маппинг старых имён → новых для production-кода

В YAML-шаблонах из § 5 (`templates/outfit-transition-v1.yaml`) и кодовых рендерерах из § 6 (`render_for_sora`, etc.) — заменить:

| Было (v1.0, Sora 2) | Стало (v1.1, Seedance 2.0) |
|---------------------|----------------------------|
| `engine: sora_2_pro` | `engine: seedance_2_0_pro` |
| `engine: sora_2_standard` | `engine: seedance_2_0_standard` |
| `def render_for_sora(template)` | `def render_for_seedance(template)` |
| OpenAI JSON storyboard schema | Seedance multi-shot text framework (см. 9.3) |
| `OPENAI_API_KEY` | `VOLCENGINE_API_KEY` |
| `https://api.openai.com/v1/videos` | `https://ark.cn-beijing.volces.com/api/v3/contents/generations/tasks` |

**Fallback chain (ОБНОВЛЁН):**

```python
ENGINE_FALLBACK = {
    "veo_3_1_standard": ["veo_3_1_fast", "kling_2_6_pro", "higgsfield"],
    "veo_3_1_fast":     ["veo_3_1_lite", "kling_2_6_std", "vidu_q3_pro"],
    "veo_3_1_lite":     ["vidu_q3_turbo", "hailuo_2_3_std"],
    "seedance_2_0_pro": ["seedance_2_0_standard", "kling_2_6_pro", "hailuo_2_3_pro"],
    "seedance_2_0_standard": ["seedance_2_0_fast", "kling_2_6_pro", "hailuo_2_3_pro"],
    "seedance_2_0_fast": ["vidu_q3_pro", "hailuo_2_3_std"],
}
```

## 9.8 5 главных подвохов AI-генерации одежды и их решение

| Подвох | Что ломается | Лучший движок | Решение |
|--------|--------------|---------------|---------|
| **1. Fabric "glow" / "ironed silk"** | Любая ткань выглядит "светящейся" | **Seedance 2.0** | physically-grounded material; добавить "tactile, hand-finished, slight wrinkles, raw cotton weave" |
| **2. Extra fingers / hand morph** | 6 пальцев, кисть-перчатка | **Veo 3.1** | "faces handled with care" + анатомия рук на close-up; кропать руки + negative prompt + YOLO-gate |
| **3. Character drift на multi-shot** | Лицо меняется shot-to-shot | **Veo 3.1** (reference lock) | "maintains appearance consistency"; для Seedance — `@Image1` + текстовый якорь черт |
| **4. Outfit morphing на движении** | Платье превращается в юбку | **Veo 3.1** для hero, **Seedance** для motion | Veo: Ingredients to Video; Seedance: `@ReferenceImage2` для outfit-lock |
| **5. Uncanny face-zoom** | Стеклянные глаза, неживые микро-выражения | **Veo 3.1** | "reducing uncanny valley, micro-expressions, breathing"; не зумить ближе medium close-up; в hook-shot НЕ доверять Seedance |
| **Бонус: Текст в кадре** | Логотипы / текст glitchят | оба плохо | НИКОГДА не генерить текст в видео; добавлять в FFmpeg post; PaddleOCR-gate |
| **Бонус: Audio drift между Veo↔Seedance** | Sync ломается на cut | — | Snimать Seedance без audio (мьютим), audio только из Veo + brand-music композитора |

## 9.9 Дополнительные источники (v1.1)

- [Ultimate prompting guide for Veo 3.1 — Google Cloud Blog](https://cloud.google.com/blog/products/ai-machine-learning/ultimate-prompting-guide-for-veo-3-1)
- [Veo 3.1 Pricing Guide 2026 — VEO3 Gen](https://www.veo3gen.app/blog/veo-3-1-pricing-plans)
- [How to Use Veo 3.1 to Generate AI Try-On Videos — aitryon.art](https://aitryon.art/blog/detail/How-to-Use-Veo-3-1-to-Generate-Stunning-AI-Try-On-Videos-b410035839c6/)
- [Fashion Reels with Veo 3 Workflow — TheNavra](https://thenavra.com/blogs/blog/go-viral-with-ai-your-simple-guide-to-making-insane-fashion-reels-with-veo-3)
- [Seedance 2.0 Official Launch — ByteDance Seed](https://seed.bytedance.com/en/blog/official-launch-of-seedance-2-0)
- [Seedance 2.0 Landing — ByteDance Seed](https://seed.bytedance.com/en/seedance2_0)
- [Seedance 2.0 Prompt Guide — Oimi AI](https://oimi.ai/en/blog/seedance-2-prompt-guide)
- [Ultimate Seedance 2.0 Prompting Guide (47 prompts) — Atlabs AI](https://www.atlabs.ai/blog/the-ultimate-seedance-2.0-prompting-guide-47-prompts-2026)
- [Awesome Seedance 2 Prompts — GitHub YouMind-OpenLab](https://github.com/YouMind-OpenLab/awesome-seedance-2-prompts)
- [Seedance 2.0 vs Veo 3.1 — MindStudio](https://www.mindstudio.ai/blog/seedance-2-vs-veo-3-1-comparison)
- [Seedance 2.0 vs Veo 3.1 — fal.ai](https://fal.ai/learn/tools/seedance-2-0-vs-veo-3-1)
- [Seedance 2.0 API Access — LaoZhang AI](https://blog.laozhang.ai/en/posts/seedance-2-api)
- [Seedance 2.0 Public Availability — GLBGPT](https://www.glbgpt.com/hub/seedance-2-0-public-availability-which-countries-have-access-api-status/)
- [Fashion Workflow Deep Dive into Nano Banana — Margaret Maynard-Reid (Medium)](https://medium.com/data-science-collective/fashion-workflow-deep-dive-into-nano-banana-3fac04584a6c)
- [Nano Banana Pro / Gemini 3 Pro Image — DeepMind](https://deepmind.google/models/gemini-image/pro/)
- [Hailuo 2.3 vs Kling v2.6 Benchmark — Vidguru](https://www.vidguru.ai/blog/hailuo-2.3-vs-kling-v2.6-comparison-benchmark.html)
- [AI Uncanny Valley in 2026 — Hailuo blog](https://hailuoai.video/pages/blog/uncanny-valley-effect-ai-video-explained)

---

**Версия документа:** 1.1 (2026-05-04)
**Статус:** ready for engineering review (передать ветке Б — бизнес-логика / RPA / LLM)
**Next steps:**
1. Удалить из § 5 / § 6 ссылки на `sora_2_*` engine, заменить по таблице 9.7
2. Запросить у Романа budget-cap для Veo 3.1 Standard quota (target $5-9k/мес)
3. Получить API key Volcengine Ark (Seedance 2.0)
4. Sprint 3 — implement `services/prompt_builder.py` с YAML-шаблоном из § 5 + dispatcher из 9.7
