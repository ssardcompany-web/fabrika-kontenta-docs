# Product Category Must-Show Library

> Что обязательно показать в кадре для каждой из 10 категорий одежды. Без этих кадров видео не продаёт.
> Применяется поверх любого формата + стиля при шаблонизации.

## Структура категории

| Параметр | Описание |
|---|---|
| `must_show_shots` | Обязательные кадры (без них видео ≠ продаёт) |
| `nice_to_have` | Кадры повышающие конверсию |
| `critical_buying_factors` | Что покупатель ОБЯЗАТЕЛЬНО хочет узнать перед покупкой |
| `red_flags_to_avoid` | Что отпугивает (показывать НЕ нужно) |
| `best_formats` | Какие форматы из 90 особенно хороши |
| `ai_difficulty` | Easy / Medium / Hard для AI-генерации |
| `platform_priority` | Где категория особенно работает |
| `seasonal_peaks` | Когда продажи пики |
| `ai_workarounds` | Как обойти проблемы AI-генерации |

---

## 1. Платья / Dresses

| Параметр | Значение |
|---|---|
| **must_show_shots** | (1) Walk-and-twirl чтобы юбка раздулась — посадка & flow ткани; (2) Sit-down — как ведёт себя при сидении; (3) Detail-zoom на материал; (4) Front + back + side профиль |
| **nice_to_have** | Подкладка / швы внутри; пуговицы / молния крупно; эффект под разным освещением; styling-варианты (с поясом / без / с жакетом) |
| **critical_buying_factors** | Длина точная (миди / макси / мини), посадка по фигуре, материал (стрейч / ригид), просвечивает ли, требует ли лифчика |
| **red_flags_to_avoid** | Слишком расплывчатый цвет (отличается от реала) → возврат; модель только в одной позе; нет вида сзади |
| **best_formats** | Walk-and-Twirl, Outfit Transition, Detail Macro, Multi-Color Showcase, Lookbook |
| **ai_difficulty** | **Hard** — fabric physics критична; шифон / шёлк особенно тяжёлый; драпировка ломается на быстром движении |
| **platform_priority** | Lamoda (если работаем) / WB / Mercado Libre + IG-органика |
| **seasonal_peaks** | Весна (свадьбы) / Лето / Праздники (декабрь) |
| **ai_workarounds** | Hybrid: реальные фото манекена + AI-фон / AI slow-motion fabric flow + реальные detail-shots ткани крупно. Kling 3 image-to-video с reference start/end frame |

---

## 2. Куртки / Outerwear

| Параметр | Значение |
|---|---|
| **must_show_shots** | (1) Sit-down с движением рук — чтобы видеть как сидит в плечах; (2) Поднять руки вверх — не задирается ли; (3) Капюшон up & down (если есть); (4) Закрыть / открыть молнию или пуговицы; (5) Front+back |
| **nice_to_have** | Карманы (внутренние и внешние); подкладка крупно; манжеты; mens vs womens cut comparison; layering (поверх свитера / т-shirt) |
| **critical_buying_factors** | Тёплость (для какой погоды), вес, защита от воды/ветра, утеплитель (синтетика / пух / wool blend), посадка (slim / regular / oversize), длина |
| **red_flags_to_avoid** | Куртка только статичная; не показано как сидит при движении; нет подкладки; не указана t° |
| **best_formats** | Real-Wear Stress-Test, Stretch Test (для трикотажных), Layering Demo, Detail-Zoom, Walk-Forward |
| **ai_difficulty** | **Medium** — менее проблемно чем платья; structured fabric стабильнее в AI |
| **platform_priority** | WB (зима!) / Ozon / Mercado Libre / IG |
| **seasonal_peaks** | Сентябрь–февраль (зима/осень) |
| **ai_workarounds** | Веo 3.1 хорошо делает structured outerwear; добавить prompt "padded jacket holds shape, no fabric distortion" |

---

## 3. Джинсы / Штаны / Брюки

| Параметр | Значение |
|---|---|
| **must_show_shots** | (1) Front + back посадка (особенно сзади — попа!); (2) Walk-forward к камере (как ложится при ходьбе); (3) Sit-down (натяжение в коленях); (4) Деталь талии (rise — high / mid / low); (5) Длина — попадает ли в обувь |
| **nice_to_have** | Карманы (особенно задние!); strech demo (растягиваются ли); 4-5 типов фигуры в одних джинсах; цвет в разном свете (indoor / outdoor) |
| **critical_buying_factors** | Размер по талии и бёдрам (русский vs us-size!), длина (для роста 165 / 170 / 175), стрейч / ригид, тип посадки (skinny / mom / wide / boyfriend) |
| **red_flags_to_avoid** | Не показано сзади (на 99% сделка ломается); нет таблицы соответствия размеров; модель идеальная (52% покупательниц с body-shape concerns) |
| **best_formats** | Multi-Body-Type, Stretch Test, Real-Wear, Walk-Forward, Body-Type Styling |
| **ai_difficulty** | **Medium-Hard** — denim хорошо в AI, но движение ткани при ходьбе требует Seedance 2 / hybrid |
| **platform_priority** | WB / Ozon (mass market) + Mercado Libre |
| **seasonal_peaks** | Весь год (базовая категория) |
| **ai_workarounds** | Veo для статики + Seedance для walk-motion. Multi-body — через AI try-on (Kling 3 / Dressie) |

---

## 4. Обувь / Footwear

| Параметр | Значение |
|---|---|
| **must_show_shots** | (1) Side-profile (главный кадр — форма!); (2) Front (toe shape); (3) Back (heel/counter); (4) Sole (подошва) — критично для качества; (5) On-foot walk — как сидит в движении; (6) Material crunching (для leather — звук) |
| **nice_to_have** | Stiching детально; logo / branding; box opening для премиум; outsole pattern макро; leather grain texture; comparison старая → новая (для thrift) |
| **critical_buying_factors** | Размер EU/RU/US (таблица обязательна!), полнота (узкая / стандарт / широкая), материал (натуральная кожа / эко-кожа / текстиль), высота каблука, sole-grip, comfort (instep / arch) |
| **red_flags_to_avoid** | Только сверху-сбоку; нет подошвы; не показана нога в обуви во время ходьбы |
| **best_formats** | 360°-Spin, Detail Macro, Walk-Forward, Box-Opening Premium, Layering (с одеждой), Stress-Test (на каблуках) |
| **ai_difficulty** | **Hard** — пальцы ноги / детали швов сложны; sole pattern AI часто галлюцинирует; крайне сложно generate consistency между shots |
| **platform_priority** | WB (mass) / Ozon / Lamoda (но они без видео) / Mercado Libre |
| **seasonal_peaks** | Сезонные пики (весна — кеды / лето — сандалии / осень — ботинки / зима — UGG-style) |
| **ai_workarounds** | Hybrid: реальные фото-обуви + AI moving model + composite в After Effects. Веo для on-foot walk; Kling для image-to-video с product photo |

---

## 5. Нижнее бельё / Lingerie / Underwear

| Параметр | Значение |
|---|---|
| **must_show_shots** | (1) Front + back на манекене или модели (нечёткое лицо для WB); (2) Detail-zoom на чашки / kanten / резинки; (3) Material zoom; (4) Plus-size demo; (5) Laundry care info (overlaid text) |
| **nice_to_have** | Layering под одеждой (как видно ткань); compatibility с разной одеждой; gift-packaging (для подарка) |
| **critical_buying_factors** | Размерная сетка (особенно лифчик — band / cup), материал (cotton / lace / microfiber), wash-rules, прозрачность ткани, push-up vs natural |
| **red_flags_to_avoid** | Слишком эротичный кадр → банится IG / WB не пропустит. Только идеализированное тело → returns 30%+ |
| **best_formats** | Detail Macro, Multi-Body-Type, Multi-Color, Texture Macro, Layering Demo |
| **ai_difficulty** | **Very Hard** — IG/TT/WB банят AI-нагoty массово; lace texture сложна; Skin proximity — uncanny valley |
| **platform_priority** | WB (нижнее бельё разрешено!) / Ozon |
| **seasonal_peaks** | Февраль (Valentine's) / Декабрь (Christmas gifting) |
| **ai_workarounds** | Только flat-lay + manequin (без модели), либо AI-only fabric & details (без лиц / тел крупно). Manequins разрешены WB для нижнего белья! |

---

## 6. Спорт / Activewear

| Параметр | Значение |
|---|---|
| **must_show_shots** | (1) Stretch test (sitting / squat / yoga pose); (2) Sweat-test (моделируем потение / показываем breathability); (3) Compression (как держит); (4) Reflective / safety элементы (для ночного бега); (5) Reverse motion (back during action) |
| **nice_to_have** | Phone-pocket (на бедре в лосинах); waistband (squat-proof); seamless construction; material technical specs (UV / antibacterial / wicking); brand-tags на одежде |
| **critical_buying_factors** | Compression уровень (light / medium / firm), stretch %, дышит ли, держит ли при движении, не просвечивает ли в squat-тесте, материал (полиэстер / нейлон / спандекс blend) |
| **red_flags_to_avoid** | Только статика на манекене; нет stretch-теста; нет реального движения; модель не потеет (нереалистично) |
| **best_formats** | Stretch Test, Real-Wear Stress-Test, Pattern Interrupt (action), Multi-Body, POV Try-On, Outfit Transition |
| **ai_difficulty** | **Medium** — material easy, но **action-physics** сложна. Seedance 2 силён в спорт-движениях |
| **platform_priority** | WB / Ozon / IG (fitness-niche органика очень сильна) |
| **seasonal_peaks** | Январь (NY resolutions) / Сентябрь (back-to-gym) |
| **ai_workarounds** | Seedance для action; Veo для static beauty; Kling для squat-test loop с reference frame |

---

## 7. Детская одежда

| Параметр | Значение |
|---|---|
| **must_show_shots** | (1) На реальном ребёнке в действии (играет / ходит); (2) Material softness (касание щекой); (3) Easy on/off (молнии / липучки крупно); (4) Care label / safety; (5) Размер по возрасту/росту overlay |
| **nice_to_have** | "Растёт с ребёнком" (для регулируемых вещей); allergy-safe материалы; school-uniform compliance (для школьной); back-to-school timing |
| **critical_buying_factors** | Размер по возрасту (74 / 80 / 86 / 92), материал (100% хлопок мечта родителя), безопасность (без вредных красителей, не линяет, не аллергенный), durability (выдержит ли стирку), удобство для ребёнка (не натирает, не давит) |
| **red_flags_to_avoid** | Кадры без ребёнка (только манекен) → не вызывают доверия; нечёткие размеры; "одежда смотрится в кадре, но в жизни плохо сидит" |
| **best_formats** | Real-Wear (action), Multi-Color, Detail-Zoom (швы / молнии), Care Guide, Layering Demo |
| **ai_difficulty** | **Very Hard** — детская внешность в AI = uncanny valley + этический риск. **Real children в кадре обязательны** (с согласием родителей) |
| **platform_priority** | WB / Ozon / Mercado Libre |
| **seasonal_peaks** | Август (back-to-school) / Декабрь (праздники) / Май-июнь (лето) |
| **ai_workarounds** | **Не генерировать детей в AI** — этический риск + платформенный бан. Только реальные дети с opt-in. AI можно использовать только для backgrounds + product flat-lay |

---

## 8. Мужская одежда

| Параметр | Значение |
|---|---|
| **must_show_shots** | (1) Front + back посадка по плечам; (2) Sit-down (брюки / костюм); (3) Walk-forward (силуэт); (4) Cuffs / collar / placket деталь; (5) Inside lining (для премиум) |
| **nice_to_have** | Watch / accessories pairing; layering (рубашка + жакет + пальто); business vs casual styling |
| **critical_buying_factors** | Размер по плечам / груди / талии, длина (рубашка не выбивается; пиджак закрывает попу), материал (cotton blend %, wool quality), посадка (slim / regular / relaxed), care ease |
| **red_flags_to_avoid** | Pull-look пиджак на модели (не его размер); только кадр спереди; нет деталей швов |
| **best_formats** | Detail Macro, Layering Demo, Walk-Forward, Body-Type Styling (худой / средний / плотный) |
| **ai_difficulty** | **Easy-Medium** — мужская одежда более structured, AI справляется лучше |
| **platform_priority** | WB / Ozon / IG (для DTC премиум) |
| **seasonal_peaks** | Март (свадебный сезон — костюмы) / Сентябрь–октябрь (back-to-office), декабрь (gifts) |
| **ai_workarounds** | Veo силён в business / formal scenes. Можно использовать для большинства форматов |

---

## 9. Аксессуары (сумки, украшения, ремни, шарфы)

| Параметр | Значение |
|---|---|
| **must_show_shots** | (1) Front / back / side; (2) Inside open (для сумок — карманы / отделения); (3) On-body (как носится — на плече / through-body); (4) Detail crystal / hardware / leather grain; (5) Box-opening (для премиум) |
| **nice_to_have** | Compatibility с разными outfits (3-5 styling вариантов); proportion-shot (на разных фигурах); usage-cases (laptop fits / phone slot) |
| **critical_buying_factors** | Размер и капакетcity (л / см); материал (натуральная кожа / saffiano / canvas); hardware quality (gold-plated / silver / matte); strap-options (regular / chain / detachable) |
| **red_flags_to_avoid** | Только flat-lay; не показано как носится на человеке |
| **best_formats** | Detail Macro, 360°-Spin (для сумок), Texture Macro, Layering (с одеждой), Box-Opening |
| **ai_difficulty** | **Easy** — аксессуары лучшие для AI! Нет лиц, hands только в кадре когда касаются — manageable |
| **platform_priority** | WB / Ozon / IG / Mercado Libre |
| **seasonal_peaks** | Декабрь (gifts) / Май (выпускные) / Сентябрь (back-to-school) |
| **ai_workarounds** | Veo / Kling Image-to-Video идеальны. Можно через "AI generates everything" 95% задач |

---

## 10. Свадебное / Вечернее

| Параметр | Значение |
|---|---|
| **must_show_shots** | (1) Walk-and-twirl с подсветкой (сияние ткани!); (2) Detail кружева / pailettes / embroidery макро; (3) Train (шлейф) — flowing during walk; (4) Back design (часто самая красивая часть); (5) Glow (закат / candlelight) |
| **nice_to_have** | Ткань в движении ветра; multiple silhouettes (mermaid / A-line / ballgown variations); accessory pairing (вуаль / fascinator); fitting-process behind-the-scenes |
| **critical_buying_factors** | Размер (особенно для on-rent), длина шлейфа, материал (silk / chiffon / lace types), sleeve-options, structure (corset / soft); back-design (low V / illusion / button-up) |
| **red_flags_to_avoid** | AI-fake glamour (uncanny valley убивает чувство мечты); только статика; нет деталей |
| **best_formats** | Walk-and-Twirl, Detail Macro, Mood-Board → Product, Atelier Walkthrough, Process video, Drop-Announcement (cinematic) |
| **ai_difficulty** | **Hard** — emotional product, slightest AI-imperfection ломает purchase. Lace / pailettes / embroidery — самые AI-fail |
| **platform_priority** | IG (главное!) / Pinterest / TikTok / Mercado Libre Clips (AR couture) |
| **seasonal_peaks** | Весна–осень (wedding season РФ); март-апрель + октябрь-декабрь (AR — южное полушарие) |
| **ai_workarounds** | Hybrid обязателен: реальная съёмка кружева + AI cinematic окружение / chandelier / cathedral. Veo 3.1 для slow-motion fabric flow. Никогда не AI лицо невесты. |

---

## Часть Б — Сводная матрица "Категория × AI-сложность"

| Категория | AI-difficulty | Workaround approach | Recommended engine |
|---|---|---|---|
| Платья | Hard | Hybrid (реал + AI) | Veo + Seedance + Kling 3 reference |
| Куртки / Outerwear | Medium | Pure AI | Veo 3.1 |
| Джинсы / Штаны | Medium-Hard | Hybrid + AI try-on | Veo + Seedance walk |
| Обувь | Hard | Hybrid (реал-фото + AI motion) | Veo + Kling reference |
| Нижнее бельё | Very Hard | Manequin + flat-lay only | Veo (без модели) |
| Спорт / Activewear | Medium | Pure AI с Seedance | Seedance 2 (action) + Veo (beauty) |
| Детская | Очень Hard (этический!) | **Только реальные дети с opt-in** | Реальные кадры + AI background |
| Мужская | Easy-Medium | Pure AI | Veo 3.1 |
| Аксессуары | **Easy** | Pure AI 95% | Veo + Kling image-to-video |
| Свадебное / Вечернее | Hard | Hybrid (реал ткань + AI окружение) | Veo (slow-mo) + реальные macro detail-shots |

## Часть В — Категория × Платформа Priority Matrix

| Категория | WB | Ozon | MELI | Lamoda | IG/TT | Pinterest | Аукционы (Mercado Libre Argentina, eBay) |
|---|---|---|---|---|---|---|---|
| Платья | ★★★ | ★★★ | ★★★ | — | ★★★★★ | ★★★★ | ★★ |
| Куртки | ★★★★★ | ★★★★ | ★★★ | — | ★★ | ★ | ★ |
| Джинсы | ★★★★★ | ★★★★★ | ★★★★ | — | ★★ | ★ | ★ |
| Обувь | ★★★★ | ★★★★ | ★★★ | — | ★★★ | ★★★ | ★★ |
| Нижнее бельё | ★★★★★ | ★★★ | ★★ | — | ★ (бан риск) | ★ | ★ |
| Спорт | ★★★★ | ★★★★ | ★★★ | — | ★★★★★ | ★★ | ★ |
| Детская | ★★★★★ | ★★★★ | ★★★ | — | ★★ | ★★★ | ★ |
| Мужская | ★★★ | ★★★ | ★★★ | — | ★★★★ | ★★ | ★ |
| Аксессуары | ★★★ | ★★★ | ★★★ | — | ★★★★★ | ★★★★★ | ★★★ |
| Свадебное | — | ★★ | ★★★ | — | ★★★★★ | ★★★★★ | ★ |

## Часть Г — INTEGRATION В YAML-ШАБЛОН

```yaml
# template.yaml (фрагмент product-секции)
product:
  category: "dresses"  # из 10 категорий
  subcategory: "evening_wear"  # дополнительно
  product_url: "{{ params.product_url }}"

  must_show_shots:
    enforce: true  # билд-обязательность
    list:
      - id: "walk_and_twirl"
        duration: 3-5s
        priority: critical
      - id: "detail_zoom_fabric"
        duration: 2-3s
        priority: critical
      - id: "back_view"
        duration: 2-3s
        priority: critical

  ai_workarounds:
    use_hybrid: true
    real_assets_required:
      - "macro_lace_detail.mp4"  # клиент загружает реальный
    ai_assets:
      - "cinematic_environment"
      - "slow_motion_fabric_flow"

  red_flags_check:
    enabled: true
    rules:
      - "must_have_back_view"  # дисплеи если нет
      - "must_show_movement_3s"
      - "no_ai_only_face_close_up"  # для wedding — face = real только

  size_table_overlay:
    enabled: true
    template: "ru_eu_us_size_chart.svg"  # стандартизированный overlay
```

## Сводная статистика

- **10 товарных категорий** × ~6 must-show кадров = **~60 обязательных типов кадров**
- **Hybrid-pipeline workarounds** для **5 категорий** (платья / обувь / нижнее бельё / детская / свадебное)
- **3 категории Easy AI** (мужская / аксессуары / куртки)
- **Категория × Платформа matrix** для приоритизации производства
- **Real-asset requirement library** для категорий где AI-only недостаточно

Готовый foundation для авто-валидации "категория-аpprapriate видео" перед deliver.
