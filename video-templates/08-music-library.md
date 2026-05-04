# Music & Audio Library

> Аудио-foundation для AI-фабрики: фоновая музыка, voice-over, лицензионная архитектура.
> Источники: Epidemic Sound 2024-2026 reports, Artlist creator usage data, ElevenLabs/Cartesia/Suno docs, TikTok Sound Library, Meta Music Rights guide.

## Часть A — БАНКИ МУЗЫКИ ПО ЛИЦЕНЗИИ

### A1. Rights-cleared подписочные (Royalty-Free)

| Сервис | Цена | Лучшее для | Кол-во треков |
|---|---|---|---|
| **Epidemic Sound** | $15/mo personal, $59/mo commercial | Универсальное, fashion-strong | 50 000+ |
| **Artlist** | $16.6/mo | Cinematic, mood-music | 20 000+ |
| **Storyblocks Audio** | $30/mo | Stock-feel, бюджетно | 100 000+ |
| **Soundstripe** | $11.99/mo | Indie / acoustic | 10 000+ |
| **Musicbed** | $20/mo | Premium cinematic | 5 000+ |
| **Audiio** | $199/year (lifetime tier) | Один платёж — навсегда | 30 000+ |
| **Mubert** | API + subscriptions | Generative AI, бесконечная | ∞ |

**Рекомендация для Фабрики Контента:** Epidemic Sound Commercial (1 аккаунт, $59/мес) + Artlist Pro ($16.6/мес) — покрывают 95% потребностей. Мubert API в дополнение для генеративного скоринга под точный BPM.

### A2. AI-генерация музыки (НОВОЕ, 2025-2026)

| Сервис | Что | Цена | Прав на коммерч. использование |
|---|---|---|---|
| **Suno AI v4** | Полные песни с вокалом по тексту | $30/mo Pro / $99/mo Premier | ✅ на платных |
| **Udio** | Аналог Suno, от ex-Google DeepMind | $30/mo Pro | ✅ на платных |
| **Stable Audio 2.0** | Open-source, инстументалки | API $0.30/min | ✅ |
| **Mubert** | Mood-based, infinite | $14/mo individual | ✅ |
| **ElevenLabs Music** | Свежий запуск 2025 | бета | TBD |
| **Beatoven.ai** | Mood + duration → custom track | $20/mo | ✅ |

**Use case для AI-фабрики:** Suno v4 для уникальных hook-треков под конкретный бренд (один трек на drop). Mubert для динамической подкладки под клиентское видео (генерация под BPM формата).

### A3. Trending TikTok Audio (особый случай)

**Правило:** на TikTok 80% reach зависит от использования trending sounds. Нельзя полагаться только на rights-cleared банки.

**Как работать:**
1. **Personal account на TikTok:** trending sound бесплатно для использования в твоих постах (TikTok-license)
2. **Business / brand account:** только pre-cleared sounds из TikTok Commercial Music Library (~150 000 треков)
3. **Spark Ads:** boostят аутентичный creator post → используют его original audio (creator должен дать разрешение через Spark code)

**Стратегия:** наши 15 stealth-сателлитов = personal accounts → trending audio доступен. Бренд-аккаунт @fabrika.contenta = limited на Commercial Library.

**Источники списков trending:**
- TikTok Creative Center (https://ads.tiktok.com/business/creativecenter/inspiration/popular/music/pc/en) — обновляется ежедневно
- Tokboard (https://tokboard.com)
- Trendpop / Pentos — paid аналитика

### A4. Бесплатные банки

- **YouTube Audio Library** — бесплатно, ограниченный выбор
- **Pixabay Music** — CC0
- **Free Music Archive (FMA)** — варьируется
- **Incompetech (Kevin MacLeod)** — CC BY (требует attribution)

Не годятся для коммерческого fashion-контента (дешёвый звук слышно).

## Часть B — МУЗЫКА ПО СТИЛИСТИКЕ (12 эстетик из Style Library)

### B1. Athleisure / Sport Performance
- **BPM:** 130–160
- **Жанры:** electronic / EDM / drum&bass / hip-hop / phonk
- **Mood:** aggressive-positive, motivational
- **Эталоны:** Imagine Dragons "Believer", The Score "Stronger", Joey Bada$$ "Devastated"
- **Эталоны на Epidemic Sound:** "Power Drive" (Hampus Naeselius), "Chase the Sun" (Loving Caliber)
- **Suno-promt:** "high-energy electronic gym track, 140 BPM, motivational drum&bass, no vocals, 30 seconds"
- **TikTok trending:** Phonk-driven (Memphis phonk, drift remixes)

### B2. Luxury Couture / Editorial
- **BPM:** 50–80
- **Жанры:** classical strings / orchestral / minimalist piano / contemporary jazz
- **Mood:** contemplative, refined
- **Эталоны:** Ludovico Einaudi "Nuvole Bianche", Max Richter "On the Nature of Daylight"
- **Эталоны на Artlist:** "Last Light" (Aaron Sprinkle), любой Hannes Bieger
- **Suno-promt:** "minimalist solo piano cinematic, 70 BPM, melancholic luxurious, no drums, evening mood"

### B3. Quiet Luxury / Stealth Wealth
- **BPM:** 60–90
- **Жанры:** ambient / minimal piano / acoustic guitar / nature soundscapes
- **Mood:** calm, refined silence
- **Эталоны:** Brian Eno "Music for Airports", Nils Frahm
- **Suno-promt:** "ambient minimal pad, 70 BPM, contemplative, soft piano with reverb, restrained, no percussion"

### B4. Streetwear / Urban Hype
- **BPM:** 90–140
- **Жанры:** hip-hop / trap / drill / lo-fi beats / boom-bap
- **Mood:** confident-aggressive
- **Эталоны:** Travis Scott "SICKO MODE", Tyler the Creator "EARFQUAKE"
- **Эталоны на Epidemic:** "Switch Up" (Jordan Halpern Schwartz)
- **Suno-promt:** "hard trap beat, 120 BPM, dark atmosphere, 808 bass, hi-hat rolls, urban"

### B5. Y2K / Trendy Pop
- **BPM:** 120–140
- **Жанры:** bubblegum pop / dance / 2000s nostalgia / hyperpop
- **Mood:** euphoric, playful
- **Эталоны:** Aqua "Barbie Girl", Britney Spears, S Club 7, FKA twigs
- **Suno-promt:** "Y2K bubblegum pop, 130 BPM, female vocals, sparkly synths, euphoric breakdown, early 2000s nostalgia"

### B6. Cottage-core / Romantic Soft
- **BPM:** 70–100
- **Жанры:** acoustic folk / indie acoustic / classical guitar / chamber music
- **Mood:** wistful-warm
- **Эталоны:** Bon Iver, Phoebe Bridgers, Iron & Wine
- **Suno-promt:** "warm acoustic folk, 85 BPM, fingerpicked guitar, gentle female harmonies, pastoral mood"

### B7. Tech-Girl / Cyber Future
- **BPM:** 110–135
- **Жанры:** synthwave / future bass / techno / K-pop
- **Mood:** futuristic-cool
- **Эталоны:** The Weeknd "Blinding Lights", Carpenter Brut, FKJ
- **Suno-promt:** "synthwave retrofuturistic, 120 BPM, gated reverb drums, neon synth lead, cyberpunk atmosphere"

### B8. Bohemian / Folk Artisan
- **BPM:** 80–110
- **Жанры:** world music / Mediterranean / Latin / ethnic strings / acoustic
- **Mood:** soulful-warm
- **Эталоны:** Buena Vista Social Club, Tinariwen, Caetano Veloso
- **Suno-promt:** "warm Mediterranean folk, 90 BPM, hand percussion, mandolin and oud, sun-drenched mood"

### B9. Workwear / Corporate Polish
- **BPM:** 90–115
- **Жанры:** cinematic instrumental / corporate-friendly jazz / soft piano / lo-fi
- **Mood:** confident-clean
- **Эталоны:** Kupla, Ratatat instrumentals
- **Suno-promt:** "polished corporate cinematic, 100 BPM, light piano with strings, professional, optimistic mid-energy"

### B10. Coastal / Resort
- **BPM:** 90–110
- **Жанры:** chillwave / tropical house / acoustic beach / bossa nova
- **Mood:** relaxed-warm
- **Эталоны:** Khruangbin, Jungle, Anitta acoustic
- **Suno-promt:** "tropical house, 100 BPM, marimba lead, beach atmosphere, sunset mood, light percussion"

### B11. Vintage 70s / 80s / 90s
- **70s BPM:** 90–120 — disco / funk / soul (Bee Gees, Earth Wind & Fire)
- **80s BPM:** 100–135 — synthwave / new wave / pop rock (Depeche Mode, A-ha)
- **90s BPM:** 90–130 — grunge / hip-hop / R&B (Nirvana, Aaliyah)
- **Suno-promt 70s:** "disco funk, 110 BPM, wah-guitar, brass section, 1976 era"
- **Suno-promt 80s:** "synthwave new wave, 125 BPM, gated reverb, FM synth, 1985 era"
- **Suno-promt 90s:** "lo-fi 90s hip-hop, 95 BPM, dusty drums, jazzy sample, J Dilla style"

### B12. Goth / Dark Academia
- **BPM:** 60–90
- **Жанры:** classical orchestral / dark ambient / cello solo / Gregorian / post-rock
- **Mood:** brooding-elegant
- **Эталоны:** Hildur Guðnadóttir, Mogwai instrumentals
- **Suno-promt:** "dark academia, 70 BPM, solo cello with orchestra strings, melancholic gothic, candlelit mood"

## Часть C — VOICE / VOICE-OVER LIBRARY

### C1. AI-голоса (TTS — Text-to-Speech)

| Сервис | Языки | Качество | Цена | Лучшее для |
|---|---|---|---|---|
| **ElevenLabs v3** | RU/ES/EN/30+ | ✅✅✅ | $5–$330/mo | Universal, лучший в emotion |
| **Cartesia (Sonic-2)** | EN/ES/много | ✅✅✅ | $49+/mo | Speed, real-time conversation |
| **Hume AI** | EN/multilang | ✅✅ | $40+/mo | Эмоция, expressive |
| **PlayHT** | RU/ES/EN | ✅✅ | $39+/mo | Long-form |
| **Sber Voice** | RU only | ✅ | по запросу | РФ-местная нативная |
| **Yandex SpeechKit** | RU only | ✅ | $1.50/1M chars | РФ-местная |
| **OpenAI TTS** | EN/multilang | ✅✅ | API pay-per-use | через VPN |
| **Suno AI** | Многоязычная | ✅✅ (поёт!) | $30+/mo | Singing voice-over |

**Рекомендация:** ElevenLabs Multilingual v2 как основной (RU + ES-AR с voseo поддерживается) + Sber Voice как fallback для РФ-нативного звучания.

### C2. Voice-tonalities по эстетике

| Стиль | Голос | Pacing | Примеры в ElevenLabs |
|---|---|---|---|
| Athleisure | Energetic male, husky female | 180–200 wpm fast | "Adam" / "Bella" |
| Luxury Couture | Calm authoritative female | 110–130 wpm slow | "Charlotte" / "Rachel" |
| Quiet Luxury | Soft confident, almost whisper | 100–120 wpm | "Lily" / "Daniel calm" |
| Streetwear | Casual urban | 150–170 wpm | "Fin" / "Charlie" |
| Y2K Pop | Bubbly female, high pitch | 170–200 wpm energetic | "Sky" / "Brittney" |
| Cottage-core | Gentle female, poetic | 90–110 wpm slow | "Rachel soft" / "Grace" |
| Tech-Girl | Robotic processed, vocoded | 130–150 wpm | "Glinda" + voice-modulation post |
| Bohemian | Warm storyteller | 110–130 wpm | "George" / "Antoni" |
| Workwear | Authoritative neutral | 130–150 wpm | "Daniel" / "Sarah" |
| Coastal | Soft warm, vacation-mode | 110–130 wpm | "Domi" / "Elli" |
| Vintage | Era-matched (radio DJ for 70s) | varies | "Ethan" + post-EQ для era-effect |
| Goth Dark | Whisper-narrative literary | 90–110 wpm | "Anto" + reverb post |

### C3. Voice cloning (для founder-аватаров)

**Use case:** клиент-DTC бренд хочет видео с голосом своего основателя, но сам он в кадре не появляется (AI-аватар).

**Процесс:**
1. Клиент даёт 1–3 минуты записи реального голоса (через WhatsApp voice memo)
2. Загружаем в ElevenLabs Voice Cloning ($22+/mo Creator tier)
3. Получаем custom-voice
4. Используем в Voice-over для всех его videos

**Юридическое:** клиент подписывает opt-in на использование клона. Деактивация по запросу за 24 часа.

### C4. Многоязычная стратегия для AR

- **ES-AR с voseo:** ElevenLabs v3 поддерживает региональные акценты. В prompt: "speaks Argentine Spanish with voseo, Buenos Aires accent"
- **Альтернатива:** реальные voice actors через Voices.com / Voice123 ($50–200/проект) — для премиум-сегмента
- **Kандидат-актёры в AR:** профессиональные narrators в Buenos Aires (доступны через локальные agency)

## Часть D — ЗВУКОВОЙ ДИЗАЙН (sound design)

### D1. Foley (footstep, fabric, ambience)

| Тип | Источник | Использование |
|---|---|---|
| Footstep | Epidemic / Boom Library | Walk-and-Twirl, fashion runway |
| Fabric rustle | Soundsnap / A Sound Effect | ASMR Fabric, slow-mo |
| Zipper | Epidemic | Garment Drop, BTS |
| Sewing machine | A Sound Effect | Atelier Walkthrough, BTS |
| Cash register | Free от Pixabay | Performance ads (FOMO) |

### D2. Whooshes & impacts (для transitions)

- **123RF Sound** — пакеты transitions $5–20
- **Big Room Sound (BRSS)** — премиум pack $200/year
- **Articulated Sounds** — kinetic typography sound effects

**Use case:** каждый transition (whip pan, glitch) сопровождается whoosh, чтобы создать "perceived motion" даже когда AI-видео не идеальное.

### D3. ASMR-комплект для премиум

- Crinkle (упаковка)
- Fabric brush (gentle)
- Box opening
- Tag snip
- Mannequin set-down

Источники: ASMR Universe pack (Storyblocks), ASMR Foley Bundle (Epidemic).

## Часть E — ИНТЕГРАЦИЯ В YAML-ШАБЛОН

```yaml
# template.yaml (фрагмент audio-секции)
audio:
  music:
    source: "epidemic_sound"
    track_id: "ES_12345"  # из API
    # ИЛИ генеративная:
    generated:
      engine: "suno_v4"
      prompt: "{{ style.suno_prompt }}"
      duration: "{{ template.duration }}"
      bpm: 130
    volume: -12dB
    fade_in: 0.5s
    fade_out: 1.0s

  voice_over:
    enabled: true
    engine: "elevenlabs_v3"
    voice_id: "EXAVITQu4vr4xnSDxMaL"  # Bella (athleisure)
    text: "{{ params.voiceover_text }}"
    language: "ru"  # ru / es-AR / en
    settings:
      stability: 0.5
      similarity_boost: 0.75
      style: 0.4
    pacing: "fast"  # fast / medium / slow
    volume: -6dB

  sfx:
    - timing: 0.0s
      type: "whoosh_in"
      file: "sfx/whoosh_01.wav"
    - timing: 5.0s
      type: "transition_glitch"
      file: "sfx/glitch_03.wav"
    - timing: 28.0s
      type: "impact_text"
      file: "sfx/impact_text.wav"

  mix:
    master_lufs: -14  # IG/TT standard
    compression: "broadcast"
    eq_preset: "vocal_clarity"
```

## Часть F — ЛИЦЕНЗИОННЫЕ ПРАВИЛА ПЛАТФОРМ

| Платформа | Музыка | Voice-over | Notes |
|---|---|---|---|
| **Instagram Reels** | Только из IG Music Library для personal; Commercial Music Library для бизнеса (~150K треков, ~50K с full rights) | Свободно (твой voice OK) | Бренд-аккаунт ограничен в trending sounds |
| **TikTok** | TikTok Sound Library для personal (включая trending); Commercial Music Library для business | Свободно | Spark Ads берёт audio с personal-creator поста |
| **YouTube Shorts** | YouTube Audio Library + любая лицензированная (Epidemic etc.) | Свободно | Самый либеральный |
| **WB видео-обложка** | **Только rights-cleared** (или без музыки) | Свободно | Нет штрафа за не-trending, поэтому только лицензия |
| **Ozon** | **БЕЗ ЗВУКА** | Не применимо | Карточки без звука по умолчанию |
| **Mercado Libre Clips** | Любая trending-OK (как TikTok) | Обязателен (звук обязателен) | Звук обязателен — иначе loop отклонят |
| **Yandex.Market** | Только rights-cleared | Свободно | Стандарт |
| **Allegro** | Rights-cleared | Свободно | Стандарт |

## Часть G — БЮДЖЕТ НА АУДИО (на 1550 видео/мес)

| Статья | Стоимость/мес |
|---|---|
| Epidemic Sound Commercial | $59 |
| Artlist Pro | $16.6 |
| ElevenLabs Creator (10K minutes) | $22 |
| Suno Pro | $30 (для уникальных треков по запросу) |
| Mubert API | $14 |
| BRSS Sound Effects | $200/year = $16/mo |
| Voice cloning slots (5 founder voices) | включено в ElevenLabs Creator |
| **Итого** | **~$158/мес** = **~15 000 ₽/мес** |

**На 1550 видео = $0.10 на аудио per video** — копейки vs $4.5 на видео-генерацию.

## Часть H — TRENDING WATCH-LIST (как мониторим тренды)

**Еженедельный flow:**
1. **Понедельник:** проверить TikTok Creative Center top 100 sounds → залистить top 10 в spreadsheet
2. **Среда:** проверить Tokboard и Pentos — какие тренды нарастают (use rate week-over-week)
3. **Пятница:** обновить music_pool.yaml для каждого сателлита-аккаунта (актуальный pre-cleared trending)

**Tools:**
- TikTok Creative Center (free)
- Tokboard ($49/mo) — глубинная аналитика трендов
- Pentos ($299/mo) — для агентств с многими брендами
- Ручной мониторинг через FYP (For You Page) каждого сателлита

## Сводная статистика

- **7 банков rights-cleared** + **6 AI-генерации** + TikTok/YouTube native pools
- **12 музыкальных профилей** (по эстетике) с BPM/жанром/Suno-promptами
- **8 TTS-сервисов** + 12 voice-tonalities + voice-cloning workflow
- **Sound design pack** (foley + whooshes + ASMR)
- **Лицензионная матрица** для 8 платформ
- **Бюджет $158/мес** на 1550 видео = $0.10/видео

Аудио-foundation готова к интеграции в YAML-pipeline.
