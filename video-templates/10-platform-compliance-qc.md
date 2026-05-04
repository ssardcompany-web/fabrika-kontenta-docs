# Platform Tech Spec + Compliance + Quality Gates

> Технические требования всех платформ + регуляторные ограничения + автоматические QC-проверки перед публикацией.
> Цель: ни одно видео не уходит к клиенту/публикации с тех-багом или compliance-нарушением.

## Часть A — TECH SPEC ПО ПЛАТФОРМАМ

### A1. Маркетплейсы

| Параметр | Wildberries | Ozon | Mercado Libre Clips | Yandex.Market | Allegro | Lamoda |
|---|---|---|---|---|---|---|
| **Длительность** | 15–60 sec (рек. 15–30) | 8–30 sec | 10 sec – 1 min | 15 sec – 2 min | до 60 sec | НЕ работаем |
| **Aspect ratio** | 9:16 (карточка) / 3:4 (autoplay) | 3:4 (одежда) / 1:1 (другое) | 9:16 | 16:9 / 1:1 | 16:9 / 9:16 | — |
| **Resolution min** | 1080p | 1080p | 1080×1920 | 1280×720 | 1280×720 | — |
| **File size max** | 1000 MB | 20–30 MB | 280 MB | 100 MB | 100 MB | — |
| **Codec** | H.264 + AAC | H.264 baseline | H.264 + AAC | H.264 | H.264 | — |
| **fps** | 30 / 60 | 30 (фикс) | 30 / 60 | 30 / 60 | 30 | — |
| **Audio** | разрешён | **БЕЗ ЗВУКА (ОБЯЗАТЕЛЬНО)** | **обязателен** | разрешён | разрешён | — |
| **Loop-able** | не требуется | **обязателен бесшовный loop** | желательно | не требуется | не требуется | — |
| **Кол-во видео на товар** | до 3 | 1 (обложка) | до 50 в карточке | до 5 | до 3 | — |
| **Captions in video** | разрешены | разрешены (но без CTA внешних) | разрешены | разрешены | разрешены | — |
| **Watermarks / logos** | **запрещены** конкуренты + bench WB-плашка | запрещены конкуренты | разрешены свои, запрещены чужие | разрешены свои | запрещены чужие | — |
| **Faces** | **полные лица модели запрещены** (нечёткие OK) | разрешены | разрешены | разрешены | разрешены | — |
| **Manequins** | **запрещены** (кроме шапок/нижнего белья/носков) | разрешены | разрешены | разрешены | разрешены | — |
| **AI-generated content** | разрешён (без явных AI-меток в кадре) | **рекомендация ≥30% реальных кадров** | разрешён | разрешён | разрешён | — |
| **Время review** | 1–2 часа | 1–24 часа | мгновенно | 1–6 часов | 1–24 часа | — |

### A2. Социальные сети — органика и реклама

| Параметр | Instagram Reels | TikTok | YouTube Shorts | Facebook Reels | VK Клипы |
|---|---|---|---|---|---|
| **Длительность** | 15–90 sec (sweet 15–30) | 15–60 sec (sweet 7–22) | до 60 sec (sweet 15–30) | 15–90 sec | до 3 min (sweet до 30) |
| **Aspect ratio** | 9:16 (1080×1920) | 9:16 (1080×1920) | 9:16 (1080×1920) | 9:16 | 9:16 / 1:1 |
| **File size max** | 4 GB | 287 MB | 256 GB | 4 GB | 2 GB |
| **Codec** | H.264 / HEVC + AAC | H.264 + AAC | H.264 / VP9 / AV1 | H.264 + AAC | H.264 + AAC |
| **fps** | 23–60 | 23–60 | 30–60 | 24–60 | 30 / 60 |
| **Audio** | обязателен (трек) | обязателен (sound) | разрешён | обязателен | разрешён |
| **Captions** | бёрнт-ин или auto-generated | бёрнт-ин или auto | auto-generated | бёрнт-ин | бёрнт-ин |
| **Safe zones (UI overlay)** | top 220px + bottom 250px | top 145px + right 140px (icons) + bottom 350px (description) | top 100px + bottom 350px | top 220px + bottom 250px | top 100px + bottom 200px |
| **Cover / Thumbnail** | пользовательский upload | первый кадр или upload | upload рекомендован | пользовательский | пользовательский |
| **Captions длина (post)** | до 2200 chars | до 2200 chars | до 100 chars (title only) | до 63 206 chars | до 16000 chars |
| **Hashtags max** | 30 (рек 5–10) | 100 (рек 3–5) | 15 в title (рек 3–5) | 30 | без жёсткого |

### A3. Платные форматы (Ads)

| Параметр | Meta Ads (FB+IG) | TikTok Ads | YouTube Ads | Yandex.Direct | VK Ads |
|---|---|---|---|---|---|
| **Длительность Reels Ads** | 1–60 sec | 5–60 sec | 6–60 sec (Bumper / In-stream) | 15–60 sec | 5–60 sec |
| **Aspect ratio** | 9:16 | 9:16 / 1:1 | 9:16 / 16:9 | 16:9 / 9:16 | 16:9 / 9:16 |
| **File size max** | 4 GB | 500 MB | 256 GB | 2 GB | 2 GB |
| **Codec** | H.264 | H.264 | H.264 / AV1 | H.264 | H.264 |
| **Captions** | обязательны 80% видео без звука | обязательны | автоматические | обязательны | обязательны |
| **CTA button** | платформенная (Shop Now / Learn More / Sign Up) | платформенная | TrueView / Bumper | визуальная | платформенная |
| **Spark Ads** | — | оригинальный creator audio + аккаунт | — | — | — |
| **Brand safety** | автомодерация | автомодерация | автомодерация | ручная + авто | автомодерация |

## Часть B — COMPLIANCE LIBRARY (что нельзя)

### B1. Запреты по платформам (универсальные)

**ВЕЗДЕ запрещено:**
- Откровенно откровенная нагота / эротика
- Алкоголь / наркотики / оружие в продакт-плейсменте
- Насилие / шок-контент
- Defamation (унижение людей или брендов)
- Чужие торговые знаки без разрешения
- Чужой контент без лицензии (música / видео / фото)

### B2. Специфика по странам

#### Россия (152-ФЗ + закон о рекламе)

- **Маркировка рекламы:** все рекламные посты с 1 сентября 2022 требуют **токена ОРД** (Оператор Рекламных Данных). Штраф за немаркированную рекламу — до 500 000 ₽ юрлицу, до 100 000 ₽ ИП.
- **152-ФЗ:** при сборе персональных данных через форму — обязательное согласие на обработку (чек-бокс). Штраф до 500 000 ₽.
- **Алкоголь:** запрещён в рекламе вне специализированных каналов
- **Финансовые услуги:** требуют лицензии
- **Закон о блогерах (с 2025):** все аккаунты с 10K+ подписчиков обязаны зарегистрироваться в Роскомнадзоре. Незарегистрированные блогеры не могут продавать рекламу легально.
- **Иноагенты:** упоминание иноагентов требует маркировки (не fashion-кейс, но знать)

#### Аргентина

- **Ley 25.326 (Personal Data Protection):** аналог GDPR/152-ФЗ. Согласие на обработку обязательно
- **Defensa del Consumidor (Ley 24.240):** реклама должна не вводить в заблуждение, цены — указаны c IVA
- **AFIP (налоговая):** инвойсы для продаж > USD 1000 обязательно с CUIT
- **Импортные таможенные правила:** при продаже импортной одежды — указание country-of-origin
- **Реклама на Meta (PAÍS-налог):** 8% surcharge на Meta Ads + 21% IVA с 2024 — учитывать в бюджетах

#### Латиноамерика общее

- **LGPD (Бразилия):** аналог GDPR
- **Mexico CFPC:** consumer protection
- **Chile Ley 21.096:** data protection

### B3. AI-content disclosure (новое 2025-2026)

**Платформенные требования:**
- **Meta (FB+IG):** с октября 2024 — обязательная метка "AI-generated" если "edited by AI" → "Made with AI" (с июля 2024 более либеральная). **Не штрафуется** но влияет на reach
- **TikTok:** с мая 2024 — auto-detection AI и обязательный label "AI-generated content". **Скрытие label = бан риск**
- **YouTube:** с марта 2024 — disclosure для "altered or synthetic content" в Creator Studio
- **LinkedIn:** воспринимает AI как обычный content

**Стратегия Stealth-сетки сателлитов:**
- Если Meta/TT детектит AI и автоматически вешает label — это OK для нас (мы Hybrid disclosure: bio упоминает "Powered by AI")
- Stealth = НЕ скрывать активно (это запрещено и кончится баном), но не выпячивать

**FTC (US):** требует disclosure для AI-influencers в коммерческой рекламе. **Не наш кейс** (РФ + AR), но полезно знать.

### B4. Маркетплейсы — специфические запреты

**Wildberries:**
- ❌ Полные лица в видео (нечёткие OK)
- ❌ Манекены (кроме шапок/белья/носков)
- ❌ Логотипы конкурентов
- ❌ Плашка "WB рекомендует" (только их собственная)
- ❌ Запрещённые товары (см. их список)
- ❌ Видео с недостоверными характеристиками (cool в любую погоду — нет, надо честно)

**Ozon:**
- ❌ Чужие торговые знаки
- ❌ Звук в видео-обложке
- ❌ Видео с явной рекламой стороннего магазина
- ❌ Изображения, не соответствующие товару

**Mercado Libre:**
- ❌ Видео без звука (отклоняется)
- ❌ Статичные кадры весь длительность (ML требует движения)
- ❌ Чужой контент / музыка без лицензии
- ❌ Выход за safe-zone UI (кнопки share/fav)

### B5. AI-генерация — что **запрещено** генерировать

**Глобально (этика + платформенные ToS):**
- Реальные узнаваемые лица знаменитостей без согласия
- Несовершеннолетних в кадре с одеждой не их возраста
- Deepfakes реальных людей без согласия
- Логотипы конкурентов
- Защищённые торговые знаки (Nike swoosh, Apple logo и т.д.)

**Юридический риск:**
- Использование клона голоса реального человека без согласия = иск возможен
- Использование лица модели без согласия = GDPR/152-ФЗ нарушение

## Часть C — QUALITY GATES (автоматические QC-проверки)

### C1. QC-чек-лист (must-pass перед deliver / publish)

```yaml
quality_gates:
  duration:
    expected: "{{ template.duration }}"
    tolerance: "±0.2s"
    fail_if_outside: true

  resolution:
    min_width: 1080
    min_height: 1920  # для 9:16
    fail_if_below: true

  fps:
    expected: 30
    tolerance: "±1"
    fail_if_outside: true

  bitrate:
    min: 8000kbps  # для 1080p H.264
    max: 50000kbps
    fail_if_outside: true

  audio:
    required: "{{ platform.audio_required }}"  # true для IG/TT, false для Ozon
    sample_rate: 48000
    channels: 2
    lufs_loudness: -14  # IG/TT broadcast standard
    peak_max: -1dBFS

  visual_qc:
    face_consistency:
      method: "face_embedding_cosine"
      threshold: 0.85  # пары первый-последний кадр
      fail_if_below: true
    fingers_anatomy:
      method: "hand_keypoint_detection"
      max_distortion: 0.15
      fail_if_above: true
    text_artifacts:
      method: "ocr_garbled_check"
      max_garble_ratio: 0.05  # max 5% non-readable text in frame
      fail_if_above: true
    color_drift:
      method: "ciede2000_brand_color"
      max_delta_e: 5  # бренд-цвета не должны сместиться > ΔE 5
      fail_if_above: true
    motion_artifacts:
      method: "ghost_double_exposure_detection"
      threshold: 0.1
      fail_if_above: true

  content_qc:
    text_in_video_ocr:
      check_for_typos: true
      check_for_competitor_brands: true
      banned_words: ["конкурент1", "конкурент2", ...]
    cta_present:
      required: "{{ template.cta.required }}"
      timing: "last_3_seconds"
    safe_zones:
      check_against_platform: true  # IG / TT / Shorts UI overlap

  compliance_qc:
    no_recognizable_celebrities: true
    no_competitor_logos: true
    no_minor_in_adult_clothing: true
    ai_disclosure_metadata:
      embedded: true  # C2PA tag
    royalty_free_audio: true  # проверка license-status
```

### C2. Автоматические инструменты QC

| Проверка | Tool | Cost |
|---|---|---|
| Face embedding consistency | InsightFace + custom Python | open-source |
| Hand keypoint detection | MediaPipe Hand | open-source |
| OCR garbled text | Tesseract / EasyOCR | open-source |
| Brand color check | OpenCV + custom | open-source |
| Loudness LUFS | FFmpeg loudnorm | open-source |
| Audio peak / clipping | FFmpeg | open-source |
| Resolution / fps / bitrate | FFprobe | open-source |
| Royalty-free audio license | API check (Epidemic / Artlist) | included in subscription |
| Safe-zone overlay | custom HTML overlay test | open-source |
| AI-detection (для disclosure) | Hive AI ($299/mo) или Sensity ($500/mo) | paid |

### C3. QC pipeline (автоматический)

```
[Video generated by Veo/Seedance/Kling]
    ↓
[FFprobe — duration, resolution, fps, bitrate]
    ↓ FAIL → reject + log → re-generate
[FFmpeg loudnorm — LUFS / peak audio]
    ↓ FAIL → re-mix audio → repeat
[InsightFace — face consistency между frame_first + frame_last]
    ↓ FAIL → reject + log → swap to image-to-video pipeline (Kling 3 с reference)
[MediaPipe Hands — fingers anatomy]
    ↓ FAIL (if hands present in frame) → reject + log → mask hands or re-prompt
[Tesseract OCR — text-in-video readability]
    ↓ FAIL → strip AI-text, replace with FFmpeg-overlay text
[Custom OpenCV — brand color delta-E]
    ↓ FAIL → apply LUT correction or re-prompt with stricter color anchor
[Hive AI / Sensity — AI-confidence score]
    ↓ logged for transparency, не блокирует
[Manual review flag — если confidence < 0.7 на любом из gates]
    ↓ человек ревьюит и approve / reject
[Approved → upload to platform via API]
    ↓
[Post-upload smoke-test — открыть live URL и убедиться что видео доступно]
```

### C4. Метрики QC pipeline

- **First-pass success rate:** target ≥60% (что не требует re-generation)
- **Average iterations to approval:** target ≤3 (от prompt до published)
- **Manual review rate:** target ≤15% (только сложные кейсы)
- **Compliance fail rate:** target 0% (после первого месяца) — все compliance-fails должны быть в pre-publish, не post

### C5. Manual review checklist (для оператора)

Если QC pipeline помечает "manual review" — оператор смотрит видео и проверяет:

- [ ] Видео реально показывает товар как ожидалось (не AI-галлюцинация другого товара)
- [ ] Лицо/руки выглядят естественно (без uncanny valley)
- [ ] Бренд-цвета совпадают с brand-guide клиента
- [ ] Текст в кадре читается и без опечаток
- [ ] CTA визуально заметен
- [ ] Звук синхронизирован с видео
- [ ] Loop seamless (для маркетплейса)
- [ ] Соответствует style-guide клиента

Время manual review: 30–60 секунд на видео. На 1550 видео/мес × 15% = 232 видео × 1 мин = ~4 часа/мес ручного ревью.

## Часть D — INTEGRATION В YAML-ШАБЛОН

```yaml
# template.yaml (фрагмент QC + compliance секции)
quality_gates:
  enabled: true
  thresholds: "{{ default_thresholds }}"  # из C1
  failures_action: "regenerate_max_3_times_then_manual_review"

compliance:
  platform: "wildberries"  # подгружает specifики A1
  country: "russia"  # для 152-ФЗ
  ai_disclosure:
    method: "bio_label"  # bio_label / metadata_only / none
  audio_license:
    source: "epidemic_sound"
    track_id: "ES_12345"
    license_proof_url: "..."

publishing:
  destination_platforms: ["wildberries", "ozon"]
  auto_adapt:
    - platform: "wildberries"
      apply: ["9:16", "no_face", "no_logo"]
    - platform: "ozon"
      apply: ["3:4", "no_audio", "loop_seamless", "30fps_strict"]
```

## Сводная статистика

- **8 маркетплейсов** + **5 социальных** + **5 ads-платформ** = **18 платформ** с tech-spec
- **3 страны** (RU + AR + LATAM) с compliance-правилами
- **30+ запретов** по AI-контенту и торговым знакам
- **15 QC-проверок** автоматических + 8 manual-checklist
- **First-pass success target 60%** + manual review 15% = production-ready pipeline

Готовый foundation для публикации без штрафов и ban'ов.
