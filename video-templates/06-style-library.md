# Style Library — 12 эстетик для AI-фабрики видео одежды

> Дата: 2026-05-04. Каждый стиль — независимый "визуальный язык".
> Один формат × один стиль = один уникальный prompt для Veo 3.1 / Seedance 2 / Kling 3.
> Источники: Vogue Business 2025-2026, Glossy.co, Business of Fashion, TikTok Creator Center, Pantone Color Reports, Modern Retail.

## Структура каждого стиля

| Параметр | Что содержит |
|---|---|
| `aesthetic_id` | Уникальный slug |
| `mood` | Настроение в 1 фразе |
| `camera` | Тип камеры + движение |
| `lighting` | Свет (тип, направление, температура) |
| `color_palette` | 5 базовых HEX + Pantone-якорь |
| `color_grade` | Финальная цветокоррекция (LUT-style) |
| `lens` | Объектив (focal length + aperture) |
| `pacing` | Скорость монтажа (cuts/sec) |
| `music_profile` | BPM / жанр / mood |
| `voice_tone` | Голос за кадром / диалог |
| `text_style` | Captions / kinetic typography |
| `composition` | Frame rules (rule of thirds, centered, dutch angle) |
| `target_brands` | Реальные бренды-эталоны |
| `target_audience` | Demographic + психографика |
| `prompt_modifiers_veo` | Готовые суффиксы для Veo 3.1 prompt |
| `prompt_modifiers_seedance` | Для Seedance 2 |
| `prompt_modifiers_kling` | Для Kling 3 (image-to-video) |
| `negative_prompts` | Что исключать |
| `valid_formats` | Какие форматы из 90 совместимы |
| `incompatible_formats` | Какие НЕ работают в этом стиле |

---

## 1. Athleisure / Sport Performance

| Параметр | Значение |
|---|---|
| **aesthetic_id** | `athleisure_dynamic` |
| **mood** | "Energy, motion, achievement" |
| **camera** | Handheld + gimbal stabilized; tracking shots; whip-pan transitions; orbit-360 на ключевых движениях |
| **lighting** | High-key bright, slight rim-light для контура мышц; daylight 5500K; иногда golden hour |
| **color_palette** | `#000000` + `#FFFFFF` + `#FF3D3D` + `#1AFC4D` + `#FFEA00`; Pantone якорь — Coral Red 17-1463 |
| **color_grade** | Vibrant + slight teal&orange (sweat highlights), high contrast, saturation +20% |
| **lens** | 24mm f/2.8 wide для action / 50mm f/1.8 для portraits / fish-eye 8mm для extreme |
| **pacing** | Rapid: 0.8–1.2 cuts/sec; sync to beat drops |
| **music_profile** | BPM 130–160; genre: electronic / hip-hop / drum&bass; mood: aggressive-positive |
| **voice_tone** | Energetic male/female; short imperative phrases ("Push.", "Move.", "Win.") |
| **text_style** | Bold sans-serif uppercase; kinetic typography с "shake" на акцентах; size 80–120pt |
| **composition** | Dutch angles в action; symmetry в beauty-shots; product centered |
| **target_brands** | Nike, Lululemon, Gymshark, Alo Yoga, Sela Sport, Adidas Originals, Bones, RECON |
| **target_audience** | 18–35, gym-goers, runners, urban athletes, weekend warriors |
| **prompt_modifiers_veo** | "high-energy gym scene, sweat-glow, motion-blur tracking, sport-focused composition, vibrant colors, fast-paced cuts" |
| **prompt_modifiers_seedance** | "athletic dynamic motion, gym setting, high-intensity action, vibrant warm tones, gimbal tracking shot" |
| **prompt_modifiers_kling** | "athletic action, motion intensity, dynamic transition, sweat detail, energetic lighting" |
| **negative_prompts** | "static, calm, dim lighting, slow-motion, vintage, soft focus, pastels" |
| **valid_formats** | Outfit Transition, Stretch Test, Real-Wear Stress-Test, Pattern Interrupt, Number-Driven Hook, Fit-Check, POV Try-On |
| **incompatible_formats** | Quiet Luxury, Mood-Board → Product, Atelier Walkthrough, Cottage-core formats |

---

## 2. Luxury Couture / Editorial

| Параметр | Значение |
|---|---|
| **aesthetic_id** | `luxury_couture_editorial` |
| **mood** | "Timeless, opulent, refined" |
| **camera** | Cinematic dolly + crane; slow tracking; static beauty shots; 24fps film-look |
| **lighting** | Chiaroscuro: 1 key light + soft fill; candlelight effects; tungsten 3200K |
| **color_palette** | `#1A1A1A` + `#D4AF37` (gold) + `#8B0000` (deep red) + `#F5F5DC` (cream) + `#2C3E50`; Pantone — Honey Gold 16-0948 |
| **color_grade** | Film LUT (Kodak Portra 400 emulation), low saturation, lifted blacks, warm shadows |
| **lens** | Anamorphic 50mm-100mm f/2.0 (cinematic bokeh); 85mm f/1.4 portrait; macro 100mm для деталей |
| **pacing** | Slow: 1 cut per 3–5 seconds; long takes; let frames breathe |
| **music_profile** | BPM 50–80; genre: classical strings / orchestral / minimalist piano / jazz; mood: contemplative |
| **voice_tone** | Whisper / authoritative female; sophisticated vocabulary; sparse |
| **text_style** | Elegant serif (Bodoni, Playfair Display); centered; pas de capitalization; gold-leaf accents |
| **composition** | Rule of thirds strict, golden ratio, negative space, vertical lines emphasized |
| **target_brands** | Dior, Chanel, Louis Vuitton, Saint Laurent, Valentino, Brunello Cucinelli, Maison Margiela, Khaite |
| **target_audience** | 30–55, HNW women, executives, collectors |
| **prompt_modifiers_veo** | "cinematic editorial, anamorphic lens, candlelight chiaroscuro, slow-motion fabric movement, marble columns, film grain Kodak Portra, warm golden hour" |
| **prompt_modifiers_seedance** | "luxury couture cinematic, slow elegant motion, opulent setting, film-style color grading, dramatic side lighting" |
| **prompt_modifiers_kling** | "couture editorial, cinematic slow motion, anamorphic flare, gold detail lighting" |
| **negative_prompts** | "fast cuts, neon, vibrant pop colors, gym setting, casual streetwear, bright daylight, oversaturated" |
| **valid_formats** | Drop-Announcement Teaser, Mood-Board → Product, Detail Macro, Manifesto, Founder Story, Anniversary, Color-of-Season Story |
| **incompatible_formats** | Athleisure formats, Y2K Trendy, Thrift Flip, Stretch Test |

---

## 3. Quiet Luxury / Stealth Wealth

| Параметр | Значение |
|---|---|
| **aesthetic_id** | `quiet_luxury_minimal` |
| **mood** | "Confidence without shouting, restraint" |
| **camera** | Static tripod 90% времени; subtle pan; никаких whip / shaking |
| **lighting** | Soft natural diffused; window light; overcast day; 5000K neutral |
| **color_palette** | `#F5F0E8` (cream) + `#A89F8E` (taupe) + `#E5E0D8` (off-white) + `#4A4039` (warm brown) + `#1A1814`; Pantone — Cloud Dancer 11-4201 |
| **color_grade** | Desaturated 70%, lifted shadows, warm tint shadows / cool tint highlights |
| **lens** | 35mm f/2.0 (natural perspective); 85mm portrait; никакого bokeh-show |
| **pacing** | Very slow: 1 cut per 4–6 seconds; static shots welcome |
| **music_profile** | BPM 60–90; genre: ambient / minimal piano / acoustic guitar / nature sounds; mood: calm |
| **voice_tone** | Soft confident; rare ASMR; whisper-narrative; minimal words |
| **text_style** | Helvetica / Inter sans-serif; small size 32–48pt; bottom-right corner; subtle |
| **composition** | Centered minimal; lots of negative space; no busy backgrounds; horizon level |
| **target_brands** | The Row, Lemaire, Khaite, Toteme, Loro Piana, Brunello Cucinelli, Jil Sander, Aritzia (Babaton line), Ushatava, 12Storeez |
| **target_audience** | 25–45, educated professionals, "old money" aspirational |
| **prompt_modifiers_veo** | "quiet luxury minimal, static composition, soft natural diffused light, neutral palette cream taupe, restrained elegance, 35mm natural perspective" |
| **prompt_modifiers_seedance** | "minimalist refined, slow contemplative motion, neutral muted tones, natural soft lighting" |
| **prompt_modifiers_kling** | "stealth wealth aesthetic, minimal composition, soft natural light, refined still" |
| **negative_prompts** | "neon, vibrant, bold colors, fast cuts, busy background, ornate, glittery, glossy, oversaturated" |
| **valid_formats** | Detail Macro, Mood-Board → Product, Pattern → Fabric → Garment, Fit-Check OOTD, Wardrobe Basics, Lookbook, Sustainability Spotlight |
| **incompatible_formats** | Y2K, Athleisure, Streetwear hyped, Goth, Trend-Jacking |

---

## 4. Streetwear / Urban Hype

| Параметр | Значение |
|---|---|
| **aesthetic_id** | `streetwear_urban` |
| **mood** | "Raw, authentic, hype, community" |
| **camera** | Handheld iPhone-aesthetic; jump cuts; unstable; natural shake |
| **lighting** | Mixed: street neon + tungsten storefront + daylight; high contrast; hard shadows OK |
| **color_palette** | `#000000` + `#FFFFFF` + `#FF6B35` + `#0066FF` + `#FFD700`; Pantone — Tangerine Tango 17-1463 |
| **color_grade** | High contrast, saturation +30%, slight green-cyan shadows (cinema look), deep blacks |
| **lens** | iPhone-equivalent 26mm; fish-eye 8mm для extreme; 35mm raw |
| **pacing** | Fast: 0.5–1.0 cuts/sec; jump cuts; rhythmic to beat |
| **music_profile** | BPM 90–140; genre: hip-hop / trap / drill / lo-fi beats; mood: confident-aggressive |
| **voice_tone** | Casual urban; slang OK; first-person speaker; energetic |
| **text_style** | Graffiti-inspired; bold display fonts (Druk, Compacta); high-contrast color blocks; sticker-pack aesthetic |
| **composition** | Asymmetric, off-center, tilted angles welcome; fill frame |
| **target_brands** | Supreme, Off-White, Stüssy, Palace, Carhartt WIP, Bape, Yeezy, Nike SB, Volchok, Gosha Rubchinskiy |
| **target_audience** | 16–28, urban youth, sneakerheads, music-driven |
| **prompt_modifiers_veo** | "streetwear urban aesthetic, iPhone handheld camera, urban location graffiti, high-contrast bold colors, jump cut energy, hard shadows" |
| **prompt_modifiers_seedance** | "urban street style, raw handheld motion, hip-hop energy, dynamic city background" |
| **prompt_modifiers_kling** | "streetwear hype, urban backdrop, raw handheld feel, bold contrast" |
| **negative_prompts** | "luxury formal, cinematic dolly, soft pastels, classical music, refined, polished, model studio" |
| **valid_formats** | Outfit Transition, Drop Countdown, UGC Honest Review, Trend-Jacking, POV Try-On, Pattern Interrupt, TikTok Spark |
| **incompatible_formats** | Quiet Luxury, Cottage-core, Workwear corporate, Luxury Couture |

---

## 5. Y2K / Trendy Pop

| Параметр | Значение |
|---|---|
| **aesthetic_id** | `y2k_trendy_pop` |
| **mood** | "Playful, chaotic, nostalgic millennium-bug aesthetic" |
| **camera** | Handheld + chaotic; sudden zooms; lens flares intentional; old camcorder aesthetic |
| **lighting** | Disco / club lighting; coloured gels (pink, blue, green); bright pop |
| **color_palette** | `#FF6B9D` + `#5DEFFF` + `#FFD93D` + `#A8E6CF` + `#9B72CF`; Pantone — Pink Carnation 14-2305 |
| **color_grade** | Hyper-saturated +40%, warm-pink shift, slight VHS noise overlay, chromatic aberration |
| **lens** | Vintage zoom 28-80mm с lens flare characteristic; soft focus halo |
| **pacing** | Very fast: 0.4–0.8 cuts/sec; chaotic; randomly inserted "freeze frames" |
| **music_profile** | BPM 120–140; genre: bubblegum pop / dance / 2000s nostalgia / hyperpop; mood: euphoric |
| **voice_tone** | Bubbly female; high pitch; OMG-vocabulary; squeal moments |
| **text_style** | Comic Sans / Curlz MT (для иронии) или modern bubblegum sans (Reckless / Editorial New); sticker-style with shadows; rainbow gradients |
| **composition** | Chaotic intentional, multiple subjects, layered emojis, busy frames welcome |
| **target_brands** | Princess Polly, Edikted, Frankie's Bikinis, Beyond Yoga, House of Sunny, Charm, Realisation Par, ZARA TRF |
| **target_audience** | 16–24, Gen Z, TikTok-native, fashion-trend chasers |
| **prompt_modifiers_veo** | "Y2K aesthetic, hyper-saturated pink and cyan, chromatic aberration, lens flares, chaotic editing energy, 2000s nostalgia, bubblegum colors" |
| **prompt_modifiers_seedance** | "Y2K trendy pop, vibrant pink cyan palette, chaotic dynamic, retro 2000s feel" |
| **prompt_modifiers_kling** | "Y2K nostalgia, hyper-saturated pop, lens flare, vintage camcorder vibe" |
| **negative_prompts** | "minimal, monochrome, slow cinematic, formal, business, restrained, neutral palette" |
| **valid_formats** | Outfit Transition, Aesthetic Challenge, Trend-Jacking, GRWM, Mirror Selfie Carousel, Reaction-First |
| **incompatible_formats** | Quiet Luxury, Workwear, Luxury Couture, Sustainability Spotlight |

---

## 6. Cottage-core / Romantic Soft

| Параметр | Значение |
|---|---|
| **aesthetic_id** | `cottagecore_romantic` |
| **mood** | "Pastoral nostalgia, gentle, feminine, dreamy" |
| **camera** | Slow gentle drift; static garden shots; soft slow-motion; natural framing through windows / flowers |
| **lighting** | Golden hour exclusively (last hour before sunset); soft window light; candles indoor |
| **color_palette** | `#F4E4D6` (peach cream) + `#D4A574` (wheat) + `#7BA582` (sage green) + `#E8B4BC` (dusty pink) + `#A0826D`; Pantone — Peach Fuzz 13-1023 |
| **color_grade** | Warm tint everything, lifted shadows, slight haze/mist filter, film grain Portra 800 |
| **lens** | 50mm f/1.4 dreamy bokeh; 85mm portrait; macro для flowers/details |
| **pacing** | Slow: 1 cut per 2–4 seconds; longer hold for atmosphere |
| **music_profile** | BPM 70–100; genre: acoustic folk / indie / classical guitar / chamber music; mood: wistful-warm |
| **voice_tone** | Gentle female narration; poetic phrasing; whisper-soft |
| **text_style** | Handwritten script (Italianno, Great Vibes); centered or scattered; small soft size; cream/blush color |
| **composition** | Through-the-window framing, floral foreground bokeh, rule of thirds, person off-center |
| **target_brands** | Reformation, Doen, Zimmermann, Lov, RIXO, Jazmin Chebar, María Cher, Cuesta Blanca, Cult Gaia, Christy Dawn |
| **target_audience** | 22–40, romantic feminine, slow-living advocates, wedding/special-occasion |
| **prompt_modifiers_veo** | "cottagecore romantic, golden hour soft light, dusty pink and sage palette, gentle slow motion, flowers in foreground bokeh, dreamy haze, film Portra grain" |
| **prompt_modifiers_seedance** | "romantic pastoral, golden warm light, soft elegant motion, wildflower setting, gentle pace" |
| **prompt_modifiers_kling** | "cottagecore aesthetic, golden hour, dreamy soft focus, romantic mood" |
| **negative_prompts** | "neon, urban, harsh shadows, fast cuts, cyber, modern minimalism, bright daylight" |
| **valid_formats** | Lookbook, Drop-Announcement, Mood-Board → Product, Color-of-Season Story, Walk-and-Twirl, Manifesto |
| **incompatible_formats** | Athleisure, Streetwear, Tech-Girl, Y2K Trendy hyped |

---

## 7. Tech-Girl / Cyber Future

| Параметр | Значение |
|---|---|
| **aesthetic_id** | `techgirl_cyber` |
| **mood** | "Futuristic, sharp, performative-cool" |
| **camera** | Smooth gimbal precision; mechanical movements; orbit; LED-array reflections |
| **lighting** | Neon backlights + LED-bars; cyan + magenta combo; dark environment with sharp accents |
| **color_palette** | `#0A0E27` (midnight) + `#00FFFF` (cyan) + `#FF00FF` (magenta) + `#FFFFFF` + `#1A1A2E`; Pantone — Galactic Cobalt 19-3950 |
| **color_grade** | High contrast, deep blacks crushed, neon highlights pushed, slight glitch effect |
| **lens** | 35mm f/1.4 sharp; anamorphic flares; dystopian wide-angle 24mm |
| **pacing** | Medium-fast: 1.0–1.5 cuts/sec; precise synchronization |
| **music_profile** | BPM 110–135; genre: synthwave / future bass / techno / Korean K-pop; mood: futuristic-cool |
| **voice_tone** | Robotic / processed; pitched / vocoded; sparse |
| **text_style** | Monospace fonts (JetBrains Mono, Space Mono); glitch animations; HUD-overlays; cyan/magenta accents |
| **composition** | Symmetrical centered, geometric patterns, leading lines (corridors), low angles |
| **target_brands** | Acne Studios, Rick Owens, Y/Project, Off-White, ADER ERROR, We11done, Mardi Mercredi, BLACKPINK collab lines |
| **target_audience** | 18–32, K-fashion enthusiasts, cyberpunk subculture, tech-savvy |
| **prompt_modifiers_veo** | "techgirl cyber aesthetic, neon cyan magenta lighting, futuristic LED corridor, anamorphic lens flare, glitch effects, dark moody high contrast" |
| **prompt_modifiers_seedance** | "cyber futuristic, neon-lit environment, mechanical precision motion, sci-fi atmosphere" |
| **prompt_modifiers_kling** | "tech-girl cyberpunk, neon glow, sharp contrast, futuristic mood" |
| **negative_prompts** | "warm tones, golden hour, cottage, pastoral, soft focus, romantic, organic textures, classical" |
| **valid_formats** | Drop-Announcement Teaser, Aesthetic Challenge, Pattern Interrupt, Outfit Transition, POV Try-On |
| **incompatible_formats** | Cottage-core, Quiet Luxury, Workwear, Sustainability |

---

## 8. Bohemian / Folk Artisan

| Параметр | Значение |
|---|---|
| **aesthetic_id** | `bohemian_folk` |
| **mood** | "Earthy, artisan, soulful, traveled" |
| **camera** | Handheld but smooth; tracking through nature; close-ups on hands working / textiles |
| **lighting** | Warm natural; candlelight; bonfire glow; sunset; tungsten warmth |
| **color_palette** | `#8B4513` (saddle brown) + `#D4A574` (wheat) + `#E07856` (terracotta) + `#3D5A4C` (forest) + `#F4E1C1` (sand); Pantone — Cinnamon Stick 18-1345 |
| **color_grade** | Earthy warm, slight desaturation but kept rich, vintage film, organic grain |
| **lens** | 50mm classic; 35mm for environment; macro для текстур handcraft |
| **pacing** | Medium-slow: 1 cut per 2–3 seconds; let textures breathe |
| **music_profile** | BPM 80–110; genre: world music / acoustic folk / Latin / Mediterranean / ethnic strings; mood: soulful |
| **voice_tone** | Warm storyteller; multilingual welcomes; poetic |
| **text_style** | Hand-lettered serif (Loubag, Cormorant); earth-tone palettes; centered; cultural ornaments |
| **composition** | Layered foreground/background, organic asymmetry, environmental portraits |
| **target_brands** | Free People, Anthropologie, Spell&Gypsy, Doen, Cuyana, Mara Hoffman, Christy Dawn, Lemlem, Ushatava (РФ), Atelier Romântico |
| **target_audience** | 25–45, sustainability-conscious, travelers, artisan-appreciators |
| **prompt_modifiers_veo** | "bohemian folk artisan, warm earthy palette, golden hour natural lighting, artisan textile detail, ethnic patterns, cinnamon and terracotta tones" |
| **prompt_modifiers_seedance** | "bohemian artisan, warm soulful lighting, organic natural motion, ethnic cultural setting" |
| **prompt_modifiers_kling** | "boho folk, warm tones, artisan craft detail, soulful warm light" |
| **negative_prompts** | "synthetic, neon, futuristic, cyber, harsh fluorescent, urban concrete, fast cuts" |
| **valid_formats** | Mood-Board → Product, Atelier Walkthrough, Pattern → Fabric → Garment, Sustainability Spotlight, Origin Story, BTS, Color-of-Season |
| **incompatible_formats** | Athleisure, Tech-Girl Cyber, Y2K Trendy |

---

## 9. Workwear / Corporate Polish

| Параметр | Значение |
|---|---|
| **aesthetic_id** | `workwear_corporate` |
| **mood** | "Professional, polished, capable" |
| **camera** | Static tripod; minimal movement; precision crops; product-focused |
| **lighting** | Clean studio softbox; even illumination; 5500K neutral; controlled shadows |
| **color_palette** | `#2C3E50` (navy) + `#F5F5F5` (off-white) + `#8B7765` (camel) + `#1A1A1A` + `#A52A2A` (burgundy accent); Pantone — Navy Blazer 19-3923 |
| **color_grade** | Neutral-natural, slight cool tint, balanced contrast, sharp focus |
| **lens** | 50mm-85mm; sharp throughout; minimal bokeh |
| **pacing** | Slow-medium: 1 cut per 3–5 seconds; clean transitions only |
| **music_profile** | BPM 90–115; genre: cinematic instrumental / corporate-friendly / soft jazz / lo-fi piano; mood: confident-clean |
| **voice_tone** | Authoritative neutral; clear articulation; trustworthy |
| **text_style** | Clean sans (Helvetica, Inter, Söhne); generous spacing; conservative size; alignment grid-strict |
| **composition** | Symmetrical, rule of thirds, professional headshots, product clean white background |
| **target_brands** | The Row, Tibi, Theory, Frame, Hugo Boss, Banana Republic, Massimo Dutti, 12Storeez (тёмная линия), Lichi, Sela formal |
| **target_audience** | 28–55, professionals, executives, business travelers |
| **prompt_modifiers_veo** | "corporate workwear professional, clean studio lighting, neutral palette navy and camel, polished composition, sharp focus, balanced symmetry" |
| **prompt_modifiers_seedance** | "professional workwear, clean composition, polished lighting, refined static motion" |
| **prompt_modifiers_kling** | "corporate professional, clean studio, polished neutral tones" |
| **negative_prompts** | "casual streetwear, neon, party, beach, athleisure, chaotic, club lighting" |
| **valid_formats** | Detail Macro, Lookbook, Wardrobe Basics, Body-Type Styling, Q&A Founder, Customer Testimonial, Capsule Wardrobe |
| **incompatible_formats** | Y2K, Streetwear hyped, Cottage-core, Tech-Girl |

---

## 10. Coastal / Resort

| Параметр | Значение |
|---|---|
| **aesthetic_id** | `coastal_resort` |
| **mood** | "Breezy, tan, vacation-luxe" |
| **camera** | Slow drift through palms / waves; slight handheld for breeze feel; aerial drone establishing shots |
| **lighting** | Bright daylight + golden hour; sun reflection on water; bounced sand light; rim light from sun |
| **color_palette** | `#F4E4C1` (sand) + `#88C0D0` (sky blue) + `#D4A574` (tan) + `#FFFFFF` + `#5D4E37` (driftwood); Pantone — Sand Storm 14-1019 |
| **color_grade** | Warm-light, slight desaturation in shadows, lifted highlights, vacation glow filter |
| **lens** | 35mm wide for environment; 85mm portraits; macro для water drops / sand details |
| **pacing** | Medium: 1 cut per 2–3 seconds; flowing |
| **music_profile** | BPM 90–110; genre: chillwave / tropical house / acoustic / bossa nova; mood: relaxed-warm |
| **voice_tone** | Soft warm female; relaxed pacing; "vacation mode" energy |
| **text_style** | Light serif (Didone) или soft sans; warm cream colors; sparse placement; minimal |
| **composition** | Horizon line strict; person small in frame for scale; layered (foreground sand, mid figure, bg ocean) |
| **target_brands** | Faithfull the Brand, Zimmermann, Frankie's Bikinis, Solid&Striped, Hunza G, Cult Gaia, Maru E Sol, Posse |
| **target_audience** | 22–45, vacation-buyers, beach-club goers, destination-wedding |
| **prompt_modifiers_veo** | "coastal resort vacation, sand and ocean palette, golden hour beach lighting, palm tree foreground, breezy slow motion, warm vacation glow" |
| **prompt_modifiers_seedance** | "coastal beach scene, warm golden light, ocean breeze motion, vacation mood" |
| **prompt_modifiers_kling** | "resort coastal, golden beach light, palm trees, breezy aesthetic" |
| **negative_prompts** | "urban concrete, neon, dark moody, cyber, indoor studio, snowy, winter" |
| **valid_formats** | Lookbook, Drop-Announcement, Walk-and-Twirl, Color-of-Season, Pop-Up Recap (event), Capsule Wardrobe |
| **incompatible_formats** | Workwear formal, Tech-Girl, Goth, Athleisure indoor |

---

## 11. Vintage 70s / 80s / 90s Retro

| Параметр | Значение |
|---|---|
| **aesthetic_id** | `vintage_retro` |
| **mood** | "Nostalgic, era-specific, warmly-faded" |
| **camera** | Era-specific movement: 70s—zooms heavy, 80s—Steadicam wide, 90s—handheld grungy; consistent fps quirk (24 with motion blur) |
| **lighting** | Era-matched: 70s warm orange tungsten; 80s neon-mixed colored; 90s harsh fluorescent or natural |
| **color_palette** | 70s: `#D4A574` + `#A52A2A` + `#3D5A4C` + `#E07856` (earth + orange + green + red); 80s: `#FF1493` + `#00FFFF` + `#FFEA00` (neon); 90s: `#5D4E37` + `#A89F8E` + `#1A1A1A` (muted muted) |
| **color_grade** | Strong era-specific film LUT (Kodak Gold 200, Fujicolor C200, VHS look); grain heavy |
| **lens** | Era-specific: 70s—50mm flat; 80s—zoom 35-105mm; 90s—28-80mm consumer |
| **pacing** | Era-specific: 70s slow disco rhythm 90 BPM; 80s mid-tempo 110; 90s grungy variable |
| **music_profile** | 70s: disco / funk / soul; 80s: synthwave / new wave / pop rock; 90s: grunge / hip-hop / R&B |
| **voice_tone** | Era-matched: 70s smooth radio DJ; 80s overly enthusiastic; 90s laidback ironic |
| **text_style** | Era-fonts: 70s—Cooper Std rounded; 80s—Pricedown / Brush Script; 90s—Trade Gothic / Times grunge |
| **composition** | 70s—centered symmetric; 80s—dynamic dutch angles; 90s—snapshots casual |
| **target_brands** | Reformation (70s line), Solid&Striped, Wrangler, Levi's vintage line, House of Sunny (70s), Diesel (90s revival) |
| **target_audience** | 22–45, nostalgia-buyers, vintage-collectors, era-aesthetic fans |
| **prompt_modifiers_veo** | "70s vintage retro, warm earth tones orange green brown, Kodak Gold 200 film grain, smooth disco era motion, sun-flare warm tungsten lighting" (адаптировать под 80s/90s) |
| **prompt_modifiers_seedance** | "vintage retro era, period-accurate styling, film grain texture, era-matched lighting" |
| **prompt_modifiers_kling** | "vintage 70s/80s/90s aesthetic, retro film look, era-styled" |
| **negative_prompts** | "modern smartphone, drone, digital noise, cyber, futuristic, contemporary minimalism" |
| **valid_formats** | Aesthetic Challenge, Lookbook, Trend-Jacking (revival), Mood-Board → Product, GRWM (era-specific), Drop-Announcement |
| **incompatible_formats** | Tech-Girl Cyber, Quiet Luxury contemporary |

---

## 12. Goth / Dark Academia

| Параметр | Значение |
|---|---|
| **aesthetic_id** | `goth_dark_academia` |
| **mood** | "Mysterious, intellectual, brooding-romantic" |
| **camera** | Slow steadicam through corridors / libraries; moody angles; static portraits; candle-lit close-ups |
| **lighting** | Low-key dramatic; single source candlelight or window; deep shadows preserved; overcast outdoors |
| **color_palette** | `#0A0A0A` (black) + `#3D2914` (dark brown) + `#722F37` (oxblood) + `#1A1A2E` (midnight) + `#8B7355` (parchment); Pantone — Bone Brown 18-1018 |
| **color_grade** | Crushed blacks, desaturated except blood-red and gold accents, vintage film grain |
| **lens** | 50mm f/1.2-1.4 deep bokeh; 85mm portrait; vintage anamorphic |
| **pacing** | Slow: 1 cut per 4–6 seconds; long takes; build tension |
| **music_profile** | BPM 60–90; genre: classical orchestral / dark ambient / cello solo / Gregorian / post-rock; mood: brooding-elegant |
| **voice_tone** | Whisper-narrative; literary references; sparse mysterious |
| **text_style** | Serif Trajan / Cinzel; centered classical; deep red/parchment colors; Latin phrases optional |
| **composition** | Centered symmetric (libraries, corridors), portrait close-ups with negative space, candlelit detail |
| **target_brands** | Rick Owens, Ann Demeulemeester, Yohji Yamamoto, Vivienne Westwood, Maison Margiela, COMME des GARÇONS, Acne Studios (dark line) |
| **target_audience** | 20–40, alternative subculture, academic aesthetic, literature-readers |
| **prompt_modifiers_veo** | "gothic dark academia, low-key candlelight, oxblood and parchment palette, slow tracking through library corridor, moody atmospheric, cathedral lighting" |
| **prompt_modifiers_seedance** | "dark academia mood, candlelit atmosphere, gothic architecture, slow brooding motion" |
| **prompt_modifiers_kling** | "goth dark academia, candle lighting, moody atmosphere, vintage portrait" |
| **negative_prompts** | "bright daylight, pastels, neon, beach, sport, cheerful, vibrant pop colors" |
| **valid_formats** | Drop-Announcement, Manifesto, Mood-Board → Product, Detail Macro, Anniversary, Color-of-Season, Lookbook |
| **incompatible_formats** | Athleisure, Coastal Resort, Y2K Pop, Cottage-core warm |

---

## Матрица "Стиль × Формат" — что совместимо

Сокращения форматов: **OT** Outfit Transition, **FC** Fit-Check, **DM** Detail Macro, **BAS** Before/After Styling, **LBK** Lookbook, **DA** Drop Announce, **MB** Mood-Board, **PAS** Problem-Agitate-Solve, **TM** Testimonial Montage, **SCN** Scarcity, **PI** Pattern Interrupt, **WT** Walk-and-Twirl, **POV** POV Try-On, **STR** Stretch Test, **360** Spin-360.

| Стиль ↓ \ Формат → | OT | FC | DM | BAS | LBK | DA | MB | PAS | TM | SCN | PI | WT | POV | STR | 360 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| **Athleisure** | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✗ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| **Luxury Couture** | ✗ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✗ | ✓ | ✗ | ✓ | ✓ | ✗ | ✗ | ✓ |
| **Quiet Luxury** | ✗ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✗ | ✓ | ✗ | ✗ | ✗ | ✗ | ✗ | ✓ |
| **Streetwear** | ✓ | ✓ | ✓ | ✓ | ✗ | ✓ | ✗ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✗ | ✗ |
| **Y2K Pop** | ✓ | ✓ | ✗ | ✓ | ✗ | ✓ | ✗ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✗ | ✗ |
| **Cottage-core** | ✗ | ✓ | ✓ | ✗ | ✓ | ✓ | ✓ | ✗ | ✓ | ✗ | ✗ | ✓ | ✗ | ✗ | ✓ |
| **Tech-Girl** | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✗ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✗ | ✓ |
| **Bohemian** | ✗ | ✓ | ✓ | ✗ | ✓ | ✓ | ✓ | ✗ | ✓ | ✗ | ✗ | ✓ | ✗ | ✗ | ✓ |
| **Workwear** | ✗ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✗ | ✗ | ✗ | ✗ | ✓ |
| **Coastal** | ✗ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✗ | ✓ | ✗ | ✗ | ✓ | ✗ | ✗ | ✓ |
| **Vintage** | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✗ | ✓ | ✗ | ✓ | ✓ | ✓ | ✗ | ✓ |
| **Goth Dark** | ✗ | ✓ | ✓ | ✗ | ✓ | ✓ | ✓ | ✗ | ✓ | ✓ | ✓ | ✓ | ✗ | ✗ | ✓ |

= **Из 90 форматов × 12 стилей теоретических 1080 комбинаций.** Валидных по матрице (✓): **~720 пригодных шаблонов** (после дальнейших экспертных фильтров — ~500 production-ready).

## Использование в YAML-шаблоне

```yaml
# template.yaml
template_id: "outfit-transition-athleisure"
format_ref: "outfit_transition"
style_ref: "athleisure_dynamic"
duration: 8s
target_platform: ["tiktok", "reels", "shorts"]

# Сценарий — наследуется из format
scenes: [...]  # см. format spec

# Стиль override — наследуется из style_ref
camera: "{{ style.camera }}"
lighting: "{{ style.lighting }}"
color_grade: "{{ style.color_grade }}"
music_profile: "{{ style.music_profile }}"
voice_tone: "{{ style.voice_tone }}"
prompt_modifiers: "{{ style.prompt_modifiers_veo }}"

# Клиент заполняет
params:
  product_url: "https://wb.ru/article/12345"
  product_category: "sportswear-leggings"
  brand_color_anchor: "#1AFC4D"  # override accent
  cta_text: "Заказать на WB"
  cta_link: "https://wb.ru/?campaign=launch&item=leggings"
```

При рендере: format_ref + style_ref + params → авто-сгенерированный prompt для Veo 3.1 / Seedance 2 / Kling 3.
