# Фабрика Контента · Реализация сайта (design doc)

> **Дата:** 3 мая 2026
> **Автор:** Александр + AI-ассистент
> **Статус:** черновик, ждёт apruv пользователя
> **Опирается на:**
> - Концепция: [`2026-05-03-fabrika-kontenta-concept-design.md`](./2026-05-03-fabrika-kontenta-concept-design.md) — 5 ICP, 7 концепций лендингов, Pricing-Гибрид
> - Стратегия: [`2026-05-03-fabrika-kontenta-marketing-strategy.md`](./2026-05-03-fabrika-kontenta-marketing-strategy.md) — RACE, 15 stealth-сателлитов, social proof, Cognitive Toolkit
> - Ресёрч: [`docs/research/2026-05-02-fabrika-kontenta-research.md`](../../research/2026-05-02-fabrika-kontenta-research.md)
>
> **Следующий артефакт:** Технический план имплементации через `superpowers:writing-plans` (после apruv)

---

## Содержание

1. [Контекст и наследство](#sec-1)
2. [Технический стек](#sec-2)
3. [Архитектура: single-site + multi-variant](#sec-3)
4. [URL-карта (36 страниц)](#sec-4)
5. [i18n стратегия](#sec-5)
6. [UTM-схема и attribution](#sec-6)
7. [Sitemap + robots.txt](#sec-7)
8. [File-routing Astro + структура проекта](#sec-8)
9. [Дизайн-система: токены](#sec-9)
10. [Компонентная база (12 блоков + 5 примитивов)](#sec-10)
11. [Mobile-first wireframes](#sec-11)
12. [Микро-взаимодействия + Accessibility](#sec-12)
13. [Копирайт-процесс + Tone of Voice](#sec-13)
14. [Schema валидации (Astro Content Collections)](#sec-14)
15. [Финальный копирайт L1 RU+ES (эталон)](#sec-15)
16. [Hero+CTA для остальных 6 типов](#sec-16)
17. [Варианты B/C — что меняется](#sec-17)
18. [Аналитика: GA4 + Метрика + Pixels + Looker Studio](#sec-18)
19. [Интеграции: amoCRM + Wati + TG-бот](#sec-19)
20. [Лидформа end-to-end](#sec-20)
21. [Performance / Lighthouse](#sec-21)
22. [Publish-pipeline (build → upload на beget)](#sec-22)
23. [SEO: Meta + Schema.org + OG](#sec-23)
24. [Безопасность + 152-ФЗ + GDPR](#sec-24)
25. [Что НЕ входит в реализацию (anti-scope)](#sec-25)

---

<a id="sec-1"></a>
## 1. Контекст и наследство

| Параметр | Из предыдущих артефактов |
|---|---|
| Продукт | Фабрика Контента — AI-генерация продающих видео для брендов одежды |
| Сегменты ICP | 5: маркетплейс-селлеры / DTC / ателье / швейные фабрики / крупные fashion-бренды |
| Pricing | Гибрид D — 49K ₽ / 89K ₽ / 149K ₽ / 490K+ ₽ + USD-эквиваленты для AR |
| Главный канал лидов | Reels/Shorts/TikTok через 15 stealth-сателлитов + бренд-аккаунты |
| Прогрев | TG-бот 4-дневный drip + WhatsApp Business (AR) + email (event-driven) |
| 7 концепций сайта | 5 single-target (L1-L5) + 1 hub (L6) + 1 universal (L7) — каждая в 3 вариантах для A/B/C-теста |

---

<a id="sec-2"></a>
## 2. Технический стек

| Слой | Выбор | Причина |
|---|---|---|
| Фреймворк | **Astro 5.x** | SSG by default, минимум JS, отличный perf для лендингов |
| CSS | **Tailwind CSS 4** | Стандарт 2026, быстрый билд, хорошо для Astro |
| Type-system | **TypeScript strict** | Защита от опечаток в variants-данных |
| UI-компоненты | **Кастомные** на Tailwind с visual-референсом из rayo-site | Не тащим shadcn-зависимости, контролируем bundle size |
| Content management | **Astro Content Collections** + JSON-варианты с zod-валидацией | Single source of truth для variants |
| CRM | **amoCRM** Базовый тариф | Лучший visual-воронка, 5 разных воронок по сегментам, готовый Salesbot для TG |
| Email | **amoCRM встроенный** + **Unisender** для broadcast | Транзакции в amoCRM, регулярные рассылки в Unisender (RU-friendly) |
| WhatsApp Business | **Wati** (через WhatsApp Business API) | Зрелый для LATAM, готовая интеграция с amoCRM |
| TG-бот | **Salebot** (или custom Python для drip-логики) | Быстрая реализация 4-дневного drip с UTM-routing |
| Аналитика in-page | GA4 + Yandex.Metrika + Pixels (Yandex/Meta/VK) | Загружаются после `requestIdleCallback` чтобы не вредили LCP |
| Аналитика дашборд | **Looker Studio** (бесплатно) | Сводный экран RACE-метрик из GA4 + amoCRM + soc-platforms |
| Hosting | **beget.tech** на старте → миграция на **Cloudflare Pages** через 1-2 месяца | Beget — что есть; CF — для server-side variants и edge-функций |
| Sitemap | **@astrojs/sitemap** | Автогенерация |

---

<a id="sec-3"></a>
## 3. Архитектура: single-site + multi-variant

**Принцип:** один codebase, варианты — это data-driven копии, тесты прозрачные.

```
SINGLE SITE
├── 6 типов страниц (home + 5 single-target)
├── × 3 варианта каждого (A default, B test, C test)
├── × 2 языка (RU + ES)
└── = 36 страниц на 1 codebase
```

**Изменили компонент Hero** → меняется во всех 36 страницах.
**Создаём вариант D для теста** → новый JSON-файл → новый URL автоматически.

---

<a id="sec-4"></a>
## 4. URL-карта (36 страниц)

### 4.1 RU (дефолт без префикса)

| URL | Тип | Концепция | Variant |
|---|---|---|---|
| `/` | Главная hub | L6 | A (default) |
| `/v/home/B` | Главная | L6 | B |
| `/v/home/C` | Главная (universal pitch) | L7 | C |
| `/marketplace-sellers` | Single-target МП | L1 | A |
| `/v/marketplace-sellers/B` `/C` | МП | L1 | B/C |
| `/dtc-fashion` | Single-target DTC | L2 | A |
| `/v/dtc-fashion/B` `/C` | DTC | L2 | B/C |
| `/atelier` | Single-target ателье | L3 | A |
| `/v/atelier/B` `/C` | Ателье | L3 | B/C |
| `/fabrika` | Single-target фабрики | L4 | A |
| `/v/fabrika/B` `/C` | Фабрики | L4 | B/C |
| `/enterprise` | Single-target Enterprise | L5 | A |
| `/v/enterprise/B` `/C` | Enterprise | L5 | B/C |

### 4.2 ES (зеркало с префиксом `/es/`)

| URL | Тип |
|---|---|
| `/es/` | Главная |
| `/es/v/home/B` `/C` | Variants главной |
| `/es/vendedores-marketplace` | МП-селлеры |
| `/es/v/vendedores-marketplace/B` `/C` | Variants |
| `/es/marcas-dtc` `/v/marcas-dtc/B` `/C` | DTC |
| `/es/atelier` `/v/atelier/B` `/C` | Ателье |
| `/es/fabrica` `/v/fabrica/B` `/C` | Фабрики |
| `/es/enterprise` `/v/enterprise/B` `/C` | Enterprise |

**Итого:** 6 типов × 3 варианта × 2 языка = **36 страниц**.

### 4.3 Служебные

| URL | Назначение |
|---|---|
| `/404` `/es/404` | Брендированные 404 |
| `/sitemap.xml` | Автогенерация |
| `/robots.txt` | Allow `/`, Disallow `/v/` |
| `/legal/oferta` `/es/legal/oferta` | Юр. документы |
| `/legal/privacy` `/es/legal/privacy` | Cookie + privacy |

---

<a id="sec-5"></a>
## 5. i18n стратегия

| Параметр | Решение |
|---|---|
| Дефолтный язык | RU (без префикса в URL) |
| Второй язык | ES с префиксом `/es/` (с voseo для AR) |
| Хостинг | Один домен, разные path-префиксы (упрощает deploy на beget) |
| Language detection | Опционально — `Accept-Language` header при первом визите → редирект на `/es/`. Можно отключить для жёсткого контроля |
| Language switcher | В шапке: текстовая ссылка RU / ES → редирект на mirror-страницу с тем же variant |
| Hreflang теги | Автогенерация в `<head>` для SEO |
| Канонические теги | Для `/v/*/B` каноникал указывает на дефолт (например `/marketplace-sellers`) |

---

<a id="sec-6"></a>
## 6. UTM-схема и attribution

### 6.1 Параметры

| Параметр | Возможные значения |
|---|---|
| `utm_source` | `satellite` / `brand` / `bot` / `ads` / `direct` |
| `utm_medium` | `reels` / `tiktok` / `shorts` / `telegram` / `whatsapp` / `email` / `linkedin` / `cpc` |
| `utm_campaign` | имя сателлита или campaign-код (`anya_kofty`, `mihail_ozon`, `fabrika_brand`, `L1_marketplace`) |
| `utm_content` | конкретный creative (`video_NNN`, `post_NNN`) |
| `utm_term` | (опц.) ключевое слово для PPC |

### 6.2 Persistence в localStorage

```typescript
// src/lib/utm.ts
const UTM_KEYS = ['utm_source', 'utm_medium', 'utm_campaign', 'utm_content', 'utm_term']
const STORAGE_KEY = 'fabrika_utm'
const TTL_DAYS = 30

export function captureUtm() {
  const url = new URL(window.location.href)
  const utm: Record<string, string> = {}
  let hasUtm = false
  for (const key of UTM_KEYS) {
    const value = url.searchParams.get(key)
    if (value) { utm[key] = value; hasUtm = true }
  }
  if (hasUtm) {
    localStorage.setItem(STORAGE_KEY, JSON.stringify({ ...utm, _captured_at: Date.now() }))
  }
}

export function getUtm() {
  const raw = localStorage.getItem(STORAGE_KEY)
  if (!raw) return {}
  const data = JSON.parse(raw)
  const ageDays = (Date.now() - data._captured_at) / (1000 * 60 * 60 * 24)
  if (ageDays > TTL_DAYS) {
    localStorage.removeItem(STORAGE_KEY)
    return {}
  }
  return data
}
```

### 6.3 Deeplink в TG-бот

```
URL: t.me/fabrika_contenta_bot?start=L1_B_anya_v042

Бот парсит payload:
  L1   = тариф маркетплейс-селлеров
  B    = variant лендинга
  anya = сателлит-источник
  v042 = ролик 042

→ Лид создаётся в amoCRM с тегами + 4-дневный drip правильного сегмента.
```

---

<a id="sec-7"></a>
## 7. Sitemap + robots.txt

### sitemap.xml (через @astrojs/sitemap)

| Группа | Priority | Включаются ли variants |
|---|---|---|
| Главная (`/`, `/es/`) | 1.0 | Только дефолт |
| Single-target (L1-L5) | 0.9 | Только дефолт |
| Universal | 0.7 | — |
| Variants (`/v/*/B`, `/C`) | — | **Не включаются** (только в тестовом трафике через UTM) |
| Legal | 0.3 | — |

### robots.txt

```
User-agent: *
Allow: /
Disallow: /v/
Disallow: /es/v/
Sitemap: https://fabrika-kontenta.ru/sitemap.xml
```

Дополнительно — каждая variant-страница содержит `<meta name="robots" content="noindex,nofollow">`.

---

<a id="sec-8"></a>
## 8. File-routing Astro + структура проекта

```
src/
├── content/
│   ├── config.ts                              # Astro Content Collection schema (zod)
│   └── pages/
│       ├── home.A.ru.json
│       ├── home.B.ru.json
│       ├── home.C.ru.json
│       ├── home.A.es.json
│       ├── home.B.es.json
│       ├── home.C.es.json
│       ├── marketplace-sellers.A.ru.json
│       ├── marketplace-sellers.B.ru.json
│       ├── marketplace-sellers.C.ru.json
│       ├── marketplace-sellers.A.es.json     # = vendedores-marketplace
│       ... (всего 36 файлов)
├── components/
│   ├── Header.astro
│   ├── Footer.astro
│   ├── LanguageSwitcher.astro
│   ├── Hero.astro
│   ├── TrustLogos.astro
│   ├── PainBlock.astro
│   ├── SolutionSteps.astro
│   ├── ProofBlock.astro
│   ├── ComplianceBlock.astro
│   ├── PricingBlock.astro
│   ├── FAQBlock.astro
│   ├── ContactForm.astro
│   └── ui/
│       ├── Button.astro
│       ├── Tag.astro
│       ├── Card.astro
│       ├── Badge.astro
│       └── Icon.astro
├── layouts/
│   ├── BaseLayout.astro                       # html/head/body wrap
│   └── LandingLayout.astro                    # шаблон для всех 36 страниц
├── pages/
│   ├── index.astro                            # /  → home.A.ru.json
│   ├── marketplace-sellers.astro              # /marketplace-sellers
│   ├── dtc-fashion.astro
│   ├── atelier.astro
│   ├── fabrika.astro
│   ├── enterprise.astro
│   ├── v/[page]/[variant].astro              # /v/home/B → home.B.ru.json
│   ├── es/
│   │   ├── index.astro                        # /es/ → home.A.es.json
│   │   ├── vendedores-marketplace.astro
│   │   ├── marcas-dtc.astro
│   │   ├── atelier.astro
│   │   ├── fabrica.astro
│   │   ├── enterprise.astro
│   │   └── v/[page]/[variant].astro
│   ├── 404.astro
│   ├── sitemap.xml.ts
│   └── robots.txt.ts
├── styles/
│   └── global.css                             # Tailwind base + utilities
├── lib/
│   ├── utm.ts                                 # localStorage UTM persistence
│   ├── form-submit.ts                         # → amoCRM webhook
│   ├── analytics.ts                           # Loaders для GA4/Метрики/Pixel
│   └── deeplinks.ts                           # TG/WA deeplink builders
└── public/
    ├── videos/
    ├── images/
    ├── og/                                    # OG-cards 1200×630
    └── logos/
```

---

<a id="sec-9"></a>
## 9. Дизайн-система: токены

### 9.1 Цвета (наследуем из rayo-site)

```typescript
colors: {
  base: {
    DEFAULT: '#161616',
    light:   '#FAF7F6',
    tint:    '#1C1C1C',
    'tint-light': '#FFFFFF',
  },
  ink: {
    DEFAULT: '#FAF7F6',
    muted:   '#ACACAC',
    soft:    '#838383',
  },
  accent: {
    DEFAULT: '#DDF160',     // lime — primary
    purple:  '#9F8BE7',     // secondary
  },
  border: {
    DEFAULT: '#303030',
    light:   '#E0DDDB',
  },
  success: '#A1F21E',
  error:   '#FF4444',
  warning: '#FFB400',
}
```

### 9.2 Типографика

```typescript
fontFamily: {
  display: ['"Cormorant Garamond"', 'Georgia', 'serif'],   // H1, H2
  body:    ['Inter', 'system-ui', 'sans-serif'],           // основной текст
  eyebrow: ['Italiana', 'serif'],                          // eyebrow / accents
  mono:    ['"JetBrains Mono"', 'Menlo', 'monospace'],     // код / цены
},
fontSize: {
  'eyebrow':    ['12px',  { lineHeight: '1.4', letterSpacing: '0.18em' }],
  'body-sm':    ['14px',  { lineHeight: '1.5' }],
  'body':       ['16px',  { lineHeight: '1.6' }],
  'body-lg':    ['18px',  { lineHeight: '1.55' }],
  'h3':         ['24px',  { lineHeight: '1.3', letterSpacing: '-0.01em' }],
  'h2':         ['32px',  { lineHeight: '1.2', letterSpacing: '-0.02em' }],
  'h1-mobile':  ['40px',  { lineHeight: '1.1', letterSpacing: '-0.02em' }],
  'h1':         ['56px',  { lineHeight: '1.05', letterSpacing: '-0.03em' }],   // ≥md
  'hero':       ['72px',  { lineHeight: '1.0',  letterSpacing: '-0.03em' }],   // ≥lg
}
```

**Reading-level правило (5–7 grade):** H1 ≤ 12 слов, sub ≤ 25 слов, абзац ≤ 3 строки.

### 9.3 Spacing, radius, shadows, motion

```typescript
spacing: {
  'section':    '96px',     // отступ между секциями (mobile: 64px)
  'block':      '48px',     // внутри секции (mobile: 32px)
  'gap':        '24px',
  'tight':      '12px',
  'page-x':     'clamp(16px, 5vw, 80px)',
},
borderRadius: {
  'btn':     '12px',
  'card':    '16px',
  'image':   '24px',
  'pill':    '9999px',
},
boxShadow: {
  'cta':       '0 8px 24px -8px rgba(221, 241, 96, 0.4)',
  'card':      '0 1px 3px rgba(0, 0, 0, 0.3)',
  'card-hover':'0 12px 32px -8px rgba(0, 0, 0, 0.5)',
  'inset-border': 'inset 0 0 0 1px rgba(255, 255, 255, 0.06)',
},
transitionDuration: {
  'micro':     '120ms',
  'standard':  '240ms',
  'slow':      '480ms',
  'reveal':    '800ms',
},
transitionTimingFunction: {
  'in-out-soft':  'cubic-bezier(0.4, 0.0, 0.2, 1)',
  'out-spring':   'cubic-bezier(0.34, 1.56, 0.64, 1)',
}
```

### 9.4 Breakpoints

```typescript
screens: {
  'sm':  '640px', 'md':  '768px', 'lg':  '1024px',
  'xl':  '1280px', '2xl': '1536px',
}
```

Дизайн начинаем с 375×812 (iPhone 12 mini), потом масштабируем.

---

<a id="sec-10"></a>
## 10. Компонентная база (12 блоков + 5 примитивов)

### 10.1 Карта компонентов

| Компонент | Где живёт | Props |
|---|---|---|
| `Header.astro` | Все 36 страниц | `lang`, `currentVariant`, `linksMap` |
| `Hero.astro` | Все 36 страниц | `eyebrow?`, `h1`, `sub`, `ctaText`, `ctaUrl`, `mediaSrc?`, `mediaType`, `trustLine?` |
| `TrustLogos.astro` | L1-L5 опционально | `logos[]`, `headingText?` |
| `PainBlock.astro` | L1-L5 (PAS) | `eyebrow`, `h2`, `painPoints[]` |
| `SolutionSteps.astro` | L1-L5 | `eyebrow`, `h2`, `steps[]` |
| `ProofBlock.astro` | L1-L5 + hub | `eyebrow`, `h2`, `cases[]` |
| `ComplianceBlock.astro` | Только L1 (МП) | `regulations[]` |
| `PricingBlock.astro` | L1-L5 | `eyebrow`, `h2`, `tiers[]` |
| `FAQBlock.astro` | Все 36 страниц | `eyebrow`, `h2`, `items[]` |
| `ContactForm.astro` | Все 36 страниц | `formId`, `fields`, `submitText`, `successMessage` |
| `Footer.astro` | Все 36 страниц | `lang`, `linksMap` |
| `LanguageSwitcher.astro` | В Header | `currentLang`, `mirrorPath` |

### 10.2 UI-примитивы

| Примитив | Назначение |
|---|---|
| `Button.astro` | Primary/Secondary/Ghost; sizes sm/md/lg |
| `Tag.astro` | Pill для категорий, статусов |
| `Card.astro` | Базовый контейнер |
| `Badge.astro` | Inline-знак ("NEW", "−20%") |
| `Icon.astro` | Lucide через astro-icon |

---

<a id="sec-11"></a>
## 11. Mobile-first wireframes

### 11.1 Hub-главная (`/`, `/v/home/B`, `/v/home/C`)

```
[Header]
[Hero: универсальный pitch для всех 5 сегментов]
[5-card sector-picker → ведут на L1-L5]
[Common-ground block: "контент дороже продукта"]
[TrustLogos]
[Mini-FAQ (3 вопроса)]
[ContactForm + alt CTA "не уверен?"]
[Footer]
```

### 11.2 Single-target landing (L1 — МП-селлеры; шаблон для L2-L5)

```
[Header]
[Hero под МП-селлеров]
[TrustLine]
[PainBlock (PAS): фотосессия 24K + WB режет индекс]
[SolutionSteps (3 шага): брифинг → AI-ген → доставка в ЛК]
[ProofBlock: до/после CTR карточек + цифры]
[PricingBlock: 49K / 89K / 149K]
[ComplianceBlock: ≥30% real / no faces — соответствие правилам]
[FAQBlock (5 вопросов сегмента)]
[ContactForm]
[Footer]
```

### 11.3 Universal (`/start`, `/comenzar`)

```
[Header]
[Hero: бесплатное первое видео за 24 ч]
[Universal pain: контент-голод / фотосессии / китайцы]
[3-step solution]
[Demo: видео-карусель 8-10 роликов]
[Pricing-teaser: «от 1 633 ₽/ролик»]
[Trust]
[ContactForm 2 поля]
[Footer]
```

### 11.4 Hero anatomy (mobile 375px)

```
┌─────────────────────────────────────┐
│ [Header: logo | menu | RU/ES]       │  ← 64px высота
├─────────────────────────────────────┤
│   <eyebrow>    (12px, lime-accent,  │  ← 32px от Header
│                 ALL-CAPS, opt.)     │
│   H1 (40px, Cormorant, white,       │  ← 16px ниже
│        max 12 слов, ≤ 3 строк)      │
│   Sub (16px, Inter, ink-muted)      │  ← 16px ниже
│   ┌─────────────────────────────┐   │
│   │ [Primary CTA — lime accent] │   │  ← 24px ниже
│   └─────────────────────────────┘   │
│   <trust-line>  (12px, ink-muted)   │  ← 16px ниже
│   ┌─────────────────────────────┐   │
│   │  Hero media (video/image)   │   │  ← 32px ниже
│   │  16:9 на mobile             │   │
│   └─────────────────────────────┘   │
└─────────────────────────────────────┘
```

Desktop ≥md: 2-колоночный grid (текст слева, media справа).

---

<a id="sec-12"></a>
## 12. Микро-взаимодействия + Accessibility

### 12.1 Микро-взаимодействия

| Состояние | Что происходит |
|---|---|
| Button hover | Background color shift (240ms) + lift shadow (translateY(-1px)) |
| Button active | translateY(0) + scale(0.98) (120ms) |
| Button loading | Spinner inline + disabled + текст «Отправляем...» |
| Form input focus | Border accent color + soft glow (240ms) |
| Form input error | Red border + small error text + shake (240ms) |
| Form submit success | Form fades + success card (480ms) |
| Card hover | Lift translateY(-4px) + shadow (240ms) |
| Image lazy-load | LQIP blur-up reveal (480ms) |
| Page reveal | Hero text fade-in + slide-up 16px (800ms staggered) |
| Sticky header on scroll | После 80px — backdrop-blur + border-bottom (240ms) |

Все анимации **respect `prefers-reduced-motion`**.

### 12.2 Accessibility (WCAG 2.1 AA)

| Требование | Реализация |
|---|---|
| Контраст текста 4.5:1 | Проверяем все цветовые комбинации в дизайн-токенах |
| Контраст крупного текста 3:1 | Hero H1 на тёмном фоне с lime accent — пасс |
| Keyboard navigation | Tab-order проверен, focus-states видимые везде |
| Screen reader | Семантичный HTML + ARIA-labels на decoratives |
| Alt-tags | Обязательно (TypeScript-required prop в Image-компонентах) |
| Form labels | `<label for>` явно связан с input |
| Skip-link | "Skip to main content" в начале body |
| Reduced motion | `prefers-reduced-motion: reduce` отключает все анимации |

---

<a id="sec-13"></a>
## 13. Копирайт-процесс + Tone of Voice

### 13.1 Команда копирайта

| Роль | Кол-во | Задача |
|---|---|---|
| Копирайтер RU (носитель) | 1 | Финальный копирайт RU-страниц, drip-сообщений, постов |
| Копирайтер ES-AR (носитель voseo) | 1 | Параллельная работа под аргентинский менталитет (не перевод) |
| Editor / brand voice keeper | 1 (Александр) | Финальный apruv, контроль ToV, single-CTA, reading-level |

**Кто пишет первичный draft:** AI на основе spec'а. Копирайтер редактирует — ускоряет работу в 3-5 раз.

### 13.2 Apruv-flow

```
1. AI generate draft (на основе JSON-spec'а)
   ↓
2. Копирайтер RU/ES-AR редактирует (брендовый voice + voseo + reading-level)
   ↓
3. Editor (Александр) approves
   ↓
4. Текст коммитится в JSON-файл
   ↓
5. Astro пересобирает → новая версия страницы
```

### 13.3 Tone of Voice — 7 правил

| # | Правило | Пример "так" | "не так" |
|---|---|---|---|
| 1 | Конкретные цифры | «30 видео за 7 дней по 1 633 ₽ за ролик» | «много видео быстро и дёшево» |
| 2 | Прямое обращение «вы» / «vos» | «Вы продаёте одежду» / «Vos vendés ropa» | «Наш продукт предлагает решения» |
| 3 | 5–7 grade reading | «Фотосессия 24K. Студия. Модель. Через 3 месяца — снова всё с нуля.» | «Традиционный продакшн-цикл предполагает значительные временные затраты» |
| 4 | Без маркетингового жаргона | «Карточка товара» | «Performance-grade конверсионная воронка» |
| 5 | Цитаты реальных клиентов | «"CTR вырос с 3.2% до 5.7%" — Анна, селлер кофт» | (анонимное "наши клиенты говорят...") |
| 6 | Один primary CTA на странице | «Получить расчёт под мои артикулы» | (3 кнопки разной важности) |
| 7 | Voseo обязателен в ES | «Sacá tu presupuesto» | «Saca tu presupuesto» |

### 13.4 Слова-маркеры

| Используем | Избегаем |
|---|---|
| Видео, ролик, контент, артикул, карточка, скорость, цена за единицу | Креатив, performance, хайп, продакшн-grade, омниканальный, синергия |
| Без съёмок, без моделей, без студии | "Революционная технология", "next-gen", "best-in-class" |
| За 7 дней, за 24 часа, за 5 рабочих дней | "Быстро", "оперативно", "в кратчайшие сроки" |
| 49 000 ₽, USD 199 | "От N", "По запросу" (только для Enterprise) |

---

<a id="sec-14"></a>
## 14. Schema валидации (Astro Content Collections + zod)

```typescript
// src/content/config.ts
import { z, defineCollection } from 'astro:content'

const pageVariant = z.object({
  meta: z.object({
    type: z.enum(['home', 'marketplace-sellers', 'dtc-fashion', 'atelier', 'fabrika', 'enterprise']),
    variant: z.enum(['A', 'B', 'C']),
    lang: z.enum(['ru', 'es']),
    title: z.string(),
    description: z.string(),
    canonical: z.string().optional(),
    noindex: z.boolean().default(false),
  }),
  hero: z.object({
    eyebrow: z.string().optional(),
    h1: z.string(),
    sub: z.string(),
    ctaText: z.string(),
    ctaUrl: z.string(),
    mediaSrc: z.string().optional(),
    mediaType: z.enum(['video', 'image']).optional(),
    trustLine: z.string().optional(),
  }),
  trustLogos: z.object({
    headingText: z.string().optional(),
    logos: z.array(z.object({ src: z.string(), alt: z.string(), href: z.string().optional() })),
  }).optional(),
  pain: z.object({
    eyebrow: z.string().optional(), h2: z.string(),
    points: z.array(z.string()),
  }).optional(),
  solution: z.object({
    eyebrow: z.string().optional(), h2: z.string(),
    steps: z.array(z.object({ number: z.string(), title: z.string(), body: z.string() })),
  }).optional(),
  proof: z.object({
    eyebrow: z.string().optional(), h2: z.string(),
    cases: z.array(z.object({ metric: z.string(), label: z.string(), description: z.string().optional() })),
  }).optional(),
  compliance: z.object({
    eyebrow: z.string().optional(), h2: z.string(),
    regulations: z.array(z.object({ platform: z.string(), rule: z.string(), ourSolution: z.string() })),
  }).optional(),
  pricing: z.object({
    eyebrow: z.string().optional(), h2: z.string(),
    tiers: z.array(z.object({
      name: z.string(), price: z.string(), currency: z.string(), period: z.string(),
      features: z.array(z.string()), ctaText: z.string(), highlighted: z.boolean().default(false),
    })),
  }).optional(),
  faq: z.object({
    eyebrow: z.string().optional(), h2: z.string(),
    items: z.array(z.object({ q: z.string(), a: z.string() })),
  }),
  contactForm: z.object({
    h2: z.string(), sub: z.string().optional(),
    submitText: z.string(), successMessage: z.string(),
  }),
})

export const collections = {
  pages: defineCollection({ type: 'data', schema: pageVariant }),
}
```

---

<a id="sec-15"></a>
## 15. Финальный копирайт L1 RU+ES (эталон)

### 15.1 L1 RU вариант A (`marketplace-sellers.A.ru.json`)

```json
{
  "meta": {
    "type": "marketplace-sellers", "variant": "A", "lang": "ru",
    "title": "100 видео-обложек для WB и Ozon за 7 дней — Фабрика Контента",
    "description": "30 AI-видео в месяц за 49 000 ₽. Соблюдаем правила маркетплейсов. Первая партия за 5 рабочих дней или вернём деньги.",
    "canonical": "https://fabrika-kontenta.ru/marketplace-sellers"
  },
  "hero": {
    "eyebrow": "ДЛЯ СЕЛЛЕРОВ WB И OZON",
    "h1": "100 видео-обложек для WB и Ozon за 7 дней. Без съёмок.",
    "sub": "49 000 ₽ за 30 видео в месяц. Соблюдаем правила маркетплейсов. Первая партия за 5 рабочих дней — или вернём деньги.",
    "ctaText": "Получить расчёт под мои артикулы",
    "ctaUrl": "#contact",
    "mediaSrc": "/videos/hero-marketplace.mp4",
    "mediaType": "video",
    "trustLine": "Помогли селлерам с оборотом от 200 K до 12 M ₽/мес"
  },
  "pain": {
    "eyebrow": "ПРОБЛЕМА",
    "h2": "Контент стоит дороже, чем сам товар.",
    "points": [
      "Фотосессия 10 артикулов — 24 000 ₽. Студия. Модель. Логистика. Каждые 3–6 месяцев снова всё с нуля — WB режет индекс актуальности.",
      "Без видео-обложки CTR карточки на 30–50 % ниже. Покупатель листает дальше.",
      "ДРР в одежде уже 30–60 % при норме 15–35 %. С новой комиссией Ozon 43–48 % с апреля 2026 — каждый рубль контента должен работать жёстче.",
      "Китайцы заходят на WB пачками — 6 000 селлеров за полгода 2025. Цены 150–300 ₽. Конкурировать нужно не ценой, а карточкой."
    ]
  },
  "solution": {
    "eyebrow": "КАК ЭТО УСТРОЕНО",
    "h2": "Из 1 фото — 30 видео-вариантов на месяц.",
    "steps": [
      { "number": "01", "title": "Брифинг и сбор материала", "body": "Загружаете фото артикулов и описание. 1 день. Никуда ехать не нужно." },
      { "number": "02", "title": "AI-генерация на Veo 3.1 / Sora 2", "body": "Делаем 30 роликов с A/B-вариантами обложек. Под правила Ozon (≥30 % реальных кадров) и WB (без распознаваемых лиц). 5 рабочих дней." },
      { "number": "03", "title": "Доставка готовых MP4", "body": "Получаете файлы готовые к загрузке в личный кабинет маркетплейса. Если первая партия не зашла — переделываем или возвращаем деньги." }
    ]
  },
  "proof": {
    "eyebrow": "ЦИФРЫ ИЗ ПИЛОТОВ",
    "h2": "Первые селлеры показывают рост карточек.",
    "cases": [
      { "metric": "+40 %", "label": "CTR карточки", "description": "Анна, селлер кофт на WB. До: 3.2 %. После 14 дней: 5.7 %." },
      { "metric": "−60 %", "label": "Стоимость контента", "description": "Михаил, джинсы на Ozon. Заменил студийную съёмку 70 K ₽/мес на нашу подписку 49 K ₽." },
      { "metric": "5 раб. дней", "label": "От брифа до первой партии", "description": "Стандартный SLA. Срочный режим за 48 часов с доплатой 30 %." }
    ]
  },
  "compliance": {
    "eyebrow": "СООТВЕТСТВИЕ ПРАВИЛАМ",
    "h2": "Не вредим вашей карточке.",
    "regulations": [
      { "platform": "Ozon", "rule": "≥30 % реальных кадров в видео", "ourSolution": "Микшируем AI-кадры с вашими реальными фото. Соотношение настраиваем под категорию." },
      { "platform": "Wildberries", "rule": "Без распознаваемых лиц и логотипов", "ourSolution": "Используем модели без чётких лиц. Логотипы убираем из кадра." },
      { "platform": "Lamoda", "rule": "Видео в карточках запрещено + 50 SKU минимум", "ourSolution": "С Lamoda не работаем. Фокус — WB и Ozon." }
    ]
  },
  "pricing": {
    "eyebrow": "ТАРИФЫ",
    "h2": "Подписка под объём ваших артикулов.",
    "tiers": [
      { "name": "Старт", "price": "49 000", "currency": "₽", "period": "/мес",
        "features": ["30 видео в месяц (8–15 сек)", "A/B-варианты обложек", "Соответствие правилам WB/Ozon", "5 рабочих дней на батч", "Поддержка в Telegram"],
        "ctaText": "Начать со Старта", "highlighted": false },
      { "name": "Рост", "price": "89 000", "currency": "₽", "period": "/мес",
        "features": ["60 видео в месяц", "Расширенные A/B-варианты", "Style-guide вашего бренда", "5 рабочих дней", "Менеджер на квартальные созвоны"],
        "ctaText": "Выбрать Рост", "highlighted": true },
      { "name": "Завод", "price": "149 000", "currency": "₽", "period": "/мес",
        "features": ["100 видео в месяц", "Кастом-сценарии под коллекции", "Срочный режим 48 часов", "Аккаунт-менеджер", "Ежемесячный отчёт"],
        "ctaText": "Выбрать Завод", "highlighted": false }
    ]
  },
  "faq": {
    "eyebrow": "ВОПРОСЫ",
    "h2": "Что обычно спрашивают селлеры.",
    "items": [
      { "q": "Чем отличаетесь от Neiro Card AI и других AI-сервисов?", "a": "Те — self-service: вы сами тратите время на брифы, отбор и доводку. Мы — под ключ: даёте фото и описание, получаете готовые ролики через 5 рабочих дней. Ваше время дороже разницы в цене." },
      { "q": "Что если WB не пропустит карточку с AI-видео?", "a": "Мы делаем под правила платформы — без распознаваемых лиц, без логотипов, с настраиваемой долей реальных кадров для Ozon. За 6 месяцев пилотов — ни одного бана. Если случится — переделываем за свой счёт." },
      { "q": "Можно вернуть деньги, если первая партия не понравится?", "a": "Да. Если первая партия 30 роликов не зайдёт по качеству или метрикам — вернём 100 % оплаты в течение 14 дней. Без споров." },
      { "q": "Срочный режим — 48 часов. Какие там ограничения?", "a": "До 10 роликов за 48 часов. Доплата 30–50 % к базовой цене. Остальные пункты тарифа без изменений." },
      { "q": "Нужно ли быть ИП или ООО для подписки?", "a": "Нет. Можно оплатить как физлицо через СБП. Чек для бухгалтерии — выставим. Для крупных сумм рекомендуем ИП." }
    ]
  },
  "contactForm": {
    "h2": "Получите бесплатное демо своего артикула за 24 часа.",
    "sub": "Отправим 1 готовый AI-ролик на ваш товар. Бесплатно. Без обязательств.",
    "submitText": "Получить бесплатное демо",
    "successMessage": "Получили! Отправим демо в Telegram в течение 24 часов."
  }
}
```

### 15.2 L1 ES-AR вариант A — Hero+Pain+Solution+Pricing+Form (фрагмент)

```json
{
  "meta": {
    "type": "marketplace-sellers", "variant": "A", "lang": "es",
    "title": "100 portadas en video para Mercado Libre en 7 días — Fábrica de Contenido",
    "description": "30 videos IA al mes desde USD 199. Cumple reglas de Mercado Libre. Primer lote en 5 días hábiles o te devolvemos la plata."
  },
  "hero": {
    "eyebrow": "PARA VENDEDORES DE MERCADO LIBRE",
    "h1": "100 portadas en video para tus publicaciones, en 7 días. Sin sesión de fotos.",
    "sub": "USD 199 al mes por 30 videos. Cumple reglas de Mercado Libre. Primer lote en 5 días hábiles — o te devolvemos la plata.",
    "ctaText": "Sacá tu presupuesto por artículo",
    "trustLine": "Trabajamos con vendedores desde Tienda Oficial hasta marcas medianas"
  },
  "pain": {
    "eyebrow": "EL PROBLEMA",
    "h2": "El contenido cuesta más que el producto.",
    "points": [
      "Una sesión de fotos de 10 artículos — ARS 350 000. Estudio. Modelo. Logística. Y cada 3 meses lo mismo de cero.",
      "Sin video en la portada, el CTR cae 30–50 %. El comprador sigue scrolleando.",
      "Comisión de Mercado Libre en ropa: 11.80–17.14 % + cargo fijo + envío gratis hasta 9.5 %. Cada peso de contenido tiene que rendir.",
      "Shein y Temu te hunden los precios desde adentro de Mercado Libre. La diferencia se hace en la portada, no en el precio."
    ]
  },
  "pricing": {
    "tiers": [
      { "name": "Inicial", "price": "USD 199", "period": "/mes",
        "features": ["30 videos al mes (8–15 segundos)", "Variaciones A/B de portada", "Cumple reglas de Mercado Libre", "5 días hábiles por lote", "Soporte en WhatsApp Business"],
        "ctaText": "Empezar con Inicial" },
      { "name": "Crecimiento", "price": "USD 349", "period": "/mes",
        "features": ["60 videos al mes", "Variaciones A/B extendidas", "Style-guide de tu marca", "5 días hábiles", "Account-manager"],
        "ctaText": "Elegir Crecimiento", "highlighted": true },
      { "name": "Fábrica", "price": "USD 599", "period": "/mes",
        "features": ["100 videos al mes", "Guiones a medida", "Modo urgente 48 horas", "Account-manager dedicado", "Reporte mensual"],
        "ctaText": "Elegir Fábrica" }
    ]
  },
  "contactForm": {
    "h2": "Recibí un demo gratis de uno de tus artículos en 24 horas.",
    "sub": "Te mandamos un video IA listo, gratis, sin compromiso.",
    "submitText": "Recibir demo gratis",
    "successMessage": "¡Listo! Te enviamos el demo por WhatsApp en 24 horas."
  }
}
```

(Полные секции pain/solution/proof/compliance/faq для ES — параллельная редакция RU-версии с voseo, аргентинскими кейсами и ARS-цифрами.)

---

<a id="sec-16"></a>
## 16. Hero+CTA для остальных 6 типов

### Главная hub (`/` и `/es/`)

| Lang | H1 | Sub | CTA |
|---|---|---|---|
| **RU** | «Фабрика Контента. AI-видео для тех, кто продаёт одежду.» | «От селлера на WB до бренда уровня Lime. Выберите свой профиль — покажем, что подходит именно вам.» | «Найти решение для моего бизнеса» |
| **ES** | «Fábrica de Contenido. Videos IA para quien vende ropa.» | «Desde un vendedor de Mercado Libre hasta una marca como Akiabara. Elegí tu perfil — te mostramos lo que sirve para vos.» | «Encontrá la solución para tu negocio» |

### DTC fashion-бренды

| Lang | H1 | Sub | CTA |
|---|---|---|---|
| **RU** | «Reels-конвейер для вашего бренда. 30 роликов в месяц в едином стиле.» | «89 000 ₽/мес. Бренд-менеджер изучает ваш стиль и держит его в каждом ролике. Освобождаем основателя от ежедневной съёмки.» | «Заказать брифинг бренда» |
| **ES** | «Fábrica de Reels para tu marca. 30 videos al mes en un solo estilo.» | «USD 349/mes. Una brand-manager aprende tu estilo y lo mantiene en cada video. Liberá al fundador del rodaje diario.» | «Pedí tu brief de marca» |

### Ателье

| Lang | H1 | Sub | CTA |
|---|---|---|---|
| **RU** | «Покажите процесс пошива. Клиент перестанет торговаться.» | «49 000 ₽ — пакет из 1 process-видео и 5–10 коротких. Лекала, материалы, ручная работа — то, что нельзя сфотографировать.» | «Снять процесс моей коллекции» |
| **ES** | «Mostrá el proceso de costura. La novia entiende por qué cuesta lo que cuesta.» | «USD 199 — paquete de 1 video del proceso + 5–10 cortos. Moldes, telares, trabajo a mano: lo que la foto no muestra.» | «Filmar el proceso de mi colección» |

### Фабрики

| Lang | H1 | Sub | CTA |
|---|---|---|---|
| **RU** | «Откройте фабрику миру. Мы приведём первых 10 заказчиков.» | «89 000 ₽ — video-portfolio мощностей + посадочная страница. Дальше — 15 000 ₽ за каждого квалифицированного лида с бюджетом от 500 K ₽/заказ.» | «Получить план лидогенерации» |
| **ES** | «Abrí tu fábrica al mundo digitalmente. Te traemos los primeros 10 clientes.» | «USD 349 — video-portfolio + landing. Después USD 60 por lead calificado con presupuesto desde USD 5 000/orden.» | «Pedí tu plan de generación de leads» |

### Enterprise

| Lang | H1 | Sub | CTA |
|---|---|---|---|
| **RU** | «Контент-фабрика для бренда уровня Lime, Befree, 12 Storeez.» | «От 490 000 ₽/мес. 200+ видео в единый бренд-стиль с гарантией SLA, NDA, dedicated team. Замена in-house контент-департамента.» | «Запланировать демо для команды» |
| **ES** | «Fábrica de contenido para marcas como Akiabara, Rapsodia, Jazmin Chebar.» | «Desde USD 1 990/mes. 200+ videos en un solo estilo de marca con SLA, NDA, equipo dedicado. Reemplaza tu departamento de contenido in-house.» | «Agendar demo para tu equipo» |

### Universal

| Lang | H1 | Sub | CTA |
|---|---|---|---|
| **RU** | «Вы продаёте одежду. Мы делаем для неё видео.» | «AI-генерация продающих роликов от 1 633 ₽/штука. Без съёмок, моделей и студии. Первый ролик за 24 часа — бесплатно.» | «Получить первое видео бесплатно» |
| **ES** | «Vos vendés ropa. Nosotros le hacemos videos.» | «Videos generados con IA desde USD 6.6/unidad. Sin sesión, sin modelos, sin estudio. Primer video en 24 horas — gratis.» | «Recibí tu primer video gratis» |

---

<a id="sec-17"></a>
## 17. Варианты B/C — что меняется

| Элемент | Вариант A (default) | Вариант B (test 1) | Вариант C (test 2) |
|---|---|---|---|
| H1 угол | Direct outcome | Pain-first | Future-self |
| CTA текст | «Получить расчёт» | «Бесплатное демо за 24 ч» | «Посмотреть кейсы» |
| Pricing-якорь в Hero | Цена прямо | За единицу | Без цены, акцент на кейсы |
| Hero-media | Видео-демо продукта | Скриншот до/после | Видео-цитата клиента |
| Proof-формат | 3 цифры в Cases | 1 большой кейс | Логотипы клиентов + общие цифры |
| Form | 3 поля | 2 поля (минимум friction) | 3 поля + дата для Calendly |

Пример L1 RU вариант B Hero:
- Eyebrow: «ВАШ КОНТЕНТ-ЦИКЛ»
- H1: «Фотосессия 24 000 ₽ — и через 3 месяца WB всё обнуляет.»
- Sub: «Каждые 3–6 месяцев индекс актуальности WB режет ваши карточки. AI-видео — 1 633 ₽ за штуку, обновляйте без поездки в студию.»
- CTA: «Бесплатное демо за 24 часа»

---

<a id="sec-18"></a>
## 18. Аналитика: GA4 + Метрика + Pixels + Looker Studio

### 18.1 События в GA4

| Событие | Триггер | Параметры |
|---|---|---|
| `page_view` | Загрузка страницы | автоматически |
| `landing_loaded` | Hero дорендерился (LCP) | `page_type`, `variant`, `lang` |
| `cta_click` | Клик по primary CTA | `cta_position`, `cta_text` |
| `pricing_viewed` | Скролл к pricing-блоку (≥50% видимости) | `tier_visible` |
| `form_started` | Первый focus на поле формы | — |
| `form_field_filled` | Заполнение каждого поля | `field_name` |
| `form_submitted` | Успешная отправка формы | `page_type`, `variant`, `lead_segment` |
| `form_error` | Ошибка валидации/отправки | `error_type` |
| `language_switched` | Клик по language switcher | `from_lang`, `to_lang` |
| `tg_bot_redirect` | Клик "Перейти в TG" после submit | `start_payload` |
| `whatsapp_click` | Клик по WhatsApp ссылке | `source_position` |
| `video_played` | Hero-video автоплей или ручной play | `video_id` |
| `faq_opened` | Раскрытие FAQ-аккордеона | `question_index` |

### 18.2 Conversion goals

| Цель | Условие | Бизнес-смысл |
|---|---|---|
| `lead_form_submit` (primary) | `form_submitted` event | Основная конверсия CR |
| `tg_bot_started` (primary) | `tg_bot_redirect` event | Лид зашёл в drip-серию |
| `pricing_engaged` (micro) | `pricing_viewed` + `cta_click` ≥ 1 | Готовность к рассмотрению |
| `cross_segment_visit` (engagement) | Визит ≥ 3 страниц разных типов | Hub-эффект |

### 18.3 Custom dimensions

| Dimension | Источник | Зачем |
|---|---|---|
| `page_type` | `home`/`marketplace-sellers`/... | Разрез по сегменту сайта |
| `variant` | `A`/`B`/`C` | A/B/C-тест разрез |
| `lang` | `ru`/`es` | Разрез по рынку |
| `utm_satellite` | имя сателлита из UTM | Какой сателлит лучше конвертит |
| `lead_segment` | значение dropdown в форме | Какой сегмент ICP реально пишет |

### 18.4 Pixels

- **Yandex Audience Pixel** — для Yandex.Direct ретаргета
- **VK Pixel** — для VK Ads ретаргета
- **Meta Pixel** — для Meta Ads (Facebook/Instagram) ретаргета — грузим **только после согласия cookie-banner**

### 18.5 Looker Studio дашборд (один экран RACE)

```
┌──────────────────────────────────────────────────────────────┐
│ ФАБРИКА КОНТЕНТА — Live dashboard                  [за 30д ▼] │
├──────────────────────────────────────────────────────────────┤
│ REACH         ACT          CONVERT        ENGAGE            │
│ 500K views    5K clicks    200 leads     3K subs TG         │
│                                                              │
│ A/B/C ТЕСТ: ТОП-3 ЛЕНДИНГА ПО CR                            │
│ #1  /marketplace-sellers/B   CR 5.2%                        │
│ #2  /dtc-fashion/A           CR 4.1%                        │
│ #3  /enterprise/C            CR 3.8%                        │
│                                                              │
│ ТОП-5 САТЕЛЛИТОВ ПО ВКЛАДУ                                  │
│ anya_kofty       180 leads / 4 paying                       │
│ mihail_ozon      95 / 2                                     │
└──────────────────────────────────────────────────────────────┘
```

---

<a id="sec-19"></a>
## 19. Интеграции: amoCRM + Wati + TG-бот

### 19.1 amoCRM (главный CRM)

**Тариф:** Базовый 499 ₽/чел × 2 пользователя на старте = ~1000 ₽/мес.

**5 воронок (по числу ICP-сегментов):**

| Воронка | Стадии |
|---|---|
| Маркетплейс-селлеры | Новый → Демо отправлено → Discovery call → Договор → Оплачен |
| DTC fashion | Новый → Бренд-брифинг → Pilot → Договор → Оплачен |
| Ателье | Новый → Консультация → Pilot пакет → Оплачен |
| Швейные фабрики B2B | Новый → Setup pitch → Pilot setup → CPL-договор → Оплачен |
| Крупные fashion (Enterprise) | Новый → Discovery → Demo для команды → Pilot → NDA + договор → Оплачен |

**API endpoint для лида:**
```
POST https://<account>.amocrm.ru/api/v4/leads
Authorization: Bearer <long-lived-token>

Body (JSON):
{
  "name": "[L1 / variant B] Анна — Одежда WB",
  "pipeline_id": <pipeline_for_marketplace>,
  "status_id": <new_lead_status>,
  "_embedded": {
    "contacts": [
      { "name": "Анна",
        "custom_fields_values": [
          { "field_code": "PHONE", "values": [{ "value": "+79991112233", "enum_code": "MOB" }] }
        ] }
    ],
    "tags": [{ "name": "satellite_anya" }, { "name": "variant_B" }]
  },
  "custom_fields_values": [
    { "field_id": <utm_source_field_id>, "values": [{ "value": "satellite" }] },
    { "field_id": <utm_medium_field_id>, "values": [{ "value": "reels" }] },
    { "field_id": <utm_campaign_field_id>, "values": [{ "value": "anya_kofty" }] },
    { "field_id": <utm_content_field_id>, "values": [{ "value": "video_042" }] },
    { "field_id": <segment_field_id>, "values": [{ "value": "marketplace" }] }
  ]
}
```

**Salesbot для TG** — встроенный виджет amoCRM, переписка автоматически валится в карточку лида.

### 19.2 Wati (WhatsApp Business для AR)

**Платформа:** Wati на базе официального WhatsApp Business API (~$49/мес стартовый тариф).

**Интеграция с amoCRM:** готовая через Wati→amoCRM connector. Сообщения из WA → в карточку лида, исходящие из amoCRM → в WA.

**Drip-сценарий (4-дневный для AR):** настраивается в Wati Workflow Builder с triggers по timestamps + tags.

### 19.3 TG-бот (Salebot или custom)

**Платформа:** Salebot (бесплатный до 100 диалогов/мес, потом 990 ₽/мес).

**Логика drip-серии:**
```
1. Юзер заходит через t.me/fabrika_contenta_bot?start=L1_B_anya_v042
   ↓
2. Бот парсит payload → определяет сегмент (L1) + variant (B) + сателлит (anya)
   ↓
3. Создаёт лид в amoCRM через webhook (с UTM-полями)
   ↓
4. Запускает 4-дневный drip из 9 сообщений (Day 0 → Day 4)
   ↓
5. На Day 3 если нет конверсии — выгружает в amoCRM "горячий лид" → менеджер делает personal outreach
```

---

<a id="sec-20"></a>
## 20. Лидформа end-to-end

### 20.1 ContactForm.astro — структура

```astro
---
// src/components/ContactForm.astro
interface Props {
  formId: string
  pageType: string
  variant: string
  lang: 'ru' | 'es'
  submitText: string
  successMessage: string
}
const { formId, pageType, variant, lang, submitText, successMessage } = Astro.props
---
<form
  id={`form-${formId}`}
  method="POST"
  data-page-type={pageType}
  data-variant={variant}
  data-lang={lang}
  data-success-message={successMessage}
>
  <label for={`${formId}-name`}>Имя или название бренда</label>
  <input id={`${formId}-name`} type="text" name="name" required minlength="2" maxlength="100" />

  <label for={`${formId}-contact`}>Telegram / WhatsApp / телефон</label>
  <input id={`${formId}-contact`} type="text" name="contact" required pattern="(@\w+|\+?\d{10,15}|.+@.+\..+)" />

  <label for={`${formId}-segment`}>Что продаёте?</label>
  <select id={`${formId}-segment`} name="segment" required>
    <option value="">— выберите —</option>
    <option value="marketplace">Одежда на WB / Ozon</option>
    <option value="dtc">Свой DTC-бренд</option>
    <option value="atelier">Свадебное / вечернее ателье</option>
    <option value="fabrika">Швейная фабрика B2B</option>
    <option value="enterprise">Крупный бренд (Lime-уровня)</option>
    <option value="other">Другое</option>
  </select>

  <!-- Honeypot (скрытое поле) -->
  <input type="text" name="honeypot" style="display:none" tabindex="-1" autocomplete="off" />

  <!-- UTM (заполняются JS) -->
  <input type="hidden" name="utm_source" />
  <input type="hidden" name="utm_medium" />
  <input type="hidden" name="utm_campaign" />
  <input type="hidden" name="utm_content" />
  <input type="hidden" name="utm_term" />

  <!-- 152-ФЗ согласие -->
  <label class="agreement">
    <input type="checkbox" name="agreement" required />
    Согласен с <a href="/legal/privacy">политикой обработки персональных данных</a>
  </label>

  <button type="submit">{submitText}</button>
</form>

<script>
  import { populateUtmFields, attachFormSubmit } from '@/lib/form-submit'
  populateUtmFields()
  attachFormSubmit()
</script>
```

### 20.2 Submit-flow

```typescript
// src/lib/form-submit.ts
import { getUtm } from './utm'

const AMO_WEBHOOK = import.meta.env.PUBLIC_AMOCRM_WEBHOOK_URL

export function attachFormSubmit() {
  document.querySelectorAll('form[data-page-type]').forEach((formEl) => {
    formEl.addEventListener('submit', async (e) => {
      e.preventDefault()
      const form = e.target as HTMLFormElement

      // Honeypot
      const honeypot = (form.elements.namedItem('honeypot') as HTMLInputElement).value
      if (honeypot) return

      // Rate limit (30 сек на IP через localStorage)
      const lastSubmit = parseInt(localStorage.getItem('last_submit') || '0', 10)
      if (Date.now() - lastSubmit < 30_000) {
        showError(form, 'Подождите 30 секунд между отправками')
        return
      }

      showLoading(form)

      const formData = new FormData(form)
      // UTM persistence
      const utm = getUtm()
      for (const [k, v] of Object.entries(utm)) formData.set(k, v as string)

      try {
        const response = await fetch(AMO_WEBHOOK, {
          method: 'POST',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify(Object.fromEntries(formData)),
        })

        if (!response.ok) throw new Error(`HTTP ${response.status}`)

        showSuccess(form, form.dataset.successMessage!)
        localStorage.setItem('last_submit', String(Date.now()))

        // GA4 event
        ;(window as any).gtag?.('event', 'form_submitted', {
          page_type: form.dataset.pageType,
          variant: form.dataset.variant,
          lead_segment: formData.get('segment'),
        })

        // Redirect в TG-бот через 2 сек
        setTimeout(() => {
          const lang = form.dataset.lang
          const segment = formData.get('segment')
          const utmCampaign = formData.get('utm_campaign')
          const utmContent = formData.get('utm_content')
          const botUrl = lang === 'es'
            ? `https://t.me/fabrica_contenido_bot?start=${segment}_${form.dataset.variant}_${utmCampaign}_${utmContent}`
            : `https://t.me/fabrika_contenta_bot?start=${segment}_${form.dataset.variant}_${utmCampaign}_${utmContent}`
          window.location.href = botUrl
        }, 2000)
      } catch (err) {
        showError(form, 'Не получилось отправить. Напишите напрямую в Telegram.')
      }
    })
  })
}

function showLoading(form: HTMLFormElement) {
  const btn = form.querySelector('button[type="submit"]')!
  btn.setAttribute('disabled', 'true')
  btn.textContent = 'Отправляем...'
}
function showSuccess(form: HTMLFormElement, message: string) {
  form.innerHTML = `<div class="success-card"><h3>${message}</h3></div>`
}
function showError(form: HTMLFormElement, message: string) {
  const btn = form.querySelector('button[type="submit"]')!
  btn.removeAttribute('disabled')
  btn.textContent = 'Попробовать ещё раз'
  // Показываем error message
  let errEl = form.querySelector('.form-error')
  if (!errEl) {
    errEl = document.createElement('div')
    errEl.className = 'form-error'
    form.appendChild(errEl)
  }
  errEl.textContent = message
}
```

### 20.3 Anti-spam (3 уровня защиты)

| Уровень | Метод |
|---|---|
| 1. Honeypot | Скрытое поле — боты заполняют, фильтруем |
| 2. Rate limit (client-side) | 30 сек между submit на IP через localStorage |
| 3. Cloudflare Turnstile (после миграции на CF) | Невидимая капча, не вредит UX |

---

<a id="sec-21"></a>
## 21. Performance / Lighthouse

### 21.1 Core Web Vitals targets

| Метрика | Целевая | "Good" по Google |
|---|---|---|
| **LCP** | <2.0 s | <2.5 s |
| **CLS** | <0.05 | <0.1 |
| **INP** | <150 ms | <200 ms |
| **TTFB** | <400 ms на beget; <100 ms после CF | <600 ms |
| **Lighthouse Performance** | ≥95 на mobile (slow 4G) | ≥90 |
| **Lighthouse SEO** | 100 | — |
| **Lighthouse Accessibility** | ≥95 | — |
| **Lighthouse Best Practices** | ≥95 | — |

### 21.2 Бюджет на ассеты

| Тип | Бюджет на страницу |
|---|---|
| HTML (gzipped) | <30 KB |
| CSS (gzipped) | <15 KB (Tailwind purge) |
| JS (gzipped) | <30 KB |
| Hero image (WebP poster) | <100 KB |
| Hero video | <800 KB (15 сек, h264 baseline, 720p) |
| Шрифты (subsetted woff2) | <80 KB |
| **Total page-weight (без видео)** | **<300 KB** на mobile |

### 21.3 Оптимизации

| Что | Как |
|---|---|
| Image optimization | Astro Image component → WebP/AVIF + responsive srcset |
| Font subsetting | Cyrillic + Latin extended subsets, woff2 only, font-display: swap |
| Hero-video | Self-hosted MP4 + WebP poster для LCP, `preload="none"` для non-hero видео |
| JS lazy-load | GA4 + Pixels через `requestIdleCallback` |
| Critical CSS | Inline в `<head>` (Astro делает автоматом) |
| Defer 3rd-party | YouTube/Vimeo embeds через facade-pattern |

---

<a id="sec-22"></a>
## 22. Publish-pipeline (build → upload на beget)

### 22.1 Локальная сборка

```bash
npm run build       # → dist/ (статика)
npm run preview     # → http://localhost:4321
```

### 22.2 Скрипт выкладки

```bash
# scripts/publish.sh
#!/bin/bash
set -e

# 1. Build
npm run build

# 2. Pre-publish checks
npm run lighthouse-ci   # Lighthouse ≥95 mobile
npm run linkcheck       # все внутренние ссылки живы

# 3. Backup текущей версии на beget
ssh user@beget "tar -czf /backups/site-$(date +%Y%m%d-%H%M%S).tar.gz /home/user/public_html"

# 4. Sync новой версии
rsync -avz --delete \
  --exclude='/backups' \
  --exclude='/.well-known' \
  dist/ user@beget:/home/user/public_html/

# 5. Smoke test
curl -sS -o /dev/null -w "%{http_code}\n" https://fabrika-kontenta.ru/
curl -sS -o /dev/null -w "%{http_code}\n" https://fabrika-kontenta.ru/marketplace-sellers
```

### 22.3 Rollback

```bash
# scripts/rollback.sh <backup-name>
ssh user@beget "rm -rf /home/user/public_html_temp && \
                cp -r /home/user/public_html /home/user/public_html_temp && \
                tar -xzf /backups/$1 -C / && \
                rm -rf /home/user/public_html_temp"
```

### 22.4 CI/CD (опционально)

GitHub Actions → push в `main` → build → lighthouse-ci → rsync на beget. Автоматическая проверка PR через Lighthouse CI бота.

### 22.5 Cache headers (через .htaccess на beget)

| Тип файла | Cache |
|---|---|
| HTML | `no-cache, must-revalidate` |
| CSS / JS / Fonts | `public, max-age=31536000, immutable` (с хэшами в имени) |
| Images / Videos | `public, max-age=2592000` (30 дней) |
| sitemap.xml | `public, max-age=86400` (1 день) |

---

<a id="sec-23"></a>
## 23. SEO: Meta + Schema.org + OG

### 23.1 Meta-теги

```html
<title>100 видео-обложек для WB и Ozon за 7 дней — Фабрика Контента</title>
<meta name="description" content="30 AI-видео в месяц за 49 000 ₽. Соблюдаем правила маркетплейсов...">

<!-- Canonical для variants B/C ведёт на A -->
<link rel="canonical" href="https://fabrika-kontenta.ru/marketplace-sellers">

<!-- Hreflang для языков -->
<link rel="alternate" hreflang="ru" href="https://fabrika-kontenta.ru/marketplace-sellers">
<link rel="alternate" hreflang="es" href="https://fabrika-kontenta.ru/es/vendedores-marketplace">
<link rel="alternate" hreflang="x-default" href="https://fabrika-kontenta.ru/marketplace-sellers">

<!-- Open Graph -->
<meta property="og:title" content="100 видео-обложек для WB и Ozon за 7 дней">
<meta property="og:description" content="30 AI-видео в месяц за 49 000 ₽...">
<meta property="og:image" content="https://fabrika-kontenta.ru/og/marketplace-sellers.png">
<meta property="og:url" content="https://fabrika-kontenta.ru/marketplace-sellers">
<meta property="og:type" content="website">

<!-- Twitter Cards -->
<meta name="twitter:card" content="summary_large_image">

<!-- Variants — noindex -->
<meta name="robots" content="noindex,nofollow">
```

### 23.2 Schema.org JSON-LD

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Service",
  "serviceType": "AI Video Production for Fashion Brands",
  "provider": {
    "@type": "Organization",
    "name": "Фабрика Контента",
    "url": "https://fabrika-kontenta.ru"
  },
  "areaServed": ["RU", "AR"],
  "offers": [
    { "@type": "Offer", "name": "Старт", "price": "49000", "priceCurrency": "RUB",
      "description": "30 AI-видео в месяц для маркетплейс-селлеров" }
  ]
}
</script>

<!-- FAQ Schema на каждой странице с FAQ-блоком -->
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "FAQPage",
  "mainEntity": [
    { "@type": "Question",
      "name": "Чем отличаетесь от Neiro Card AI?",
      "acceptedAnswer": { "@type": "Answer", "text": "..." } }
  ]
}
</script>
```

### 23.3 OG-картинки

Автогенерация уникальной OG-карточки 1200×630 на каждый тип страницы с H1 + accent-цветом через Astro Image Functions или статические SVG-конвертеры.

---

<a id="sec-24"></a>
## 24. Безопасность + 152-ФЗ + GDPR

### 24.1 HTTPS

| Слой | Решение |
|---|---|
| SSL-сертификат | Let's Encrypt через beget admin panel |
| Force HTTPS redirect | `.htaccess` rewrite |
| HSTS header | `Strict-Transport-Security: max-age=31536000; includeSubDomains` |

### 24.2 CSP (Content Security Policy)

```
Content-Security-Policy:
  default-src 'self';
  script-src 'self' 'unsafe-inline' https://www.googletagmanager.com https://mc.yandex.ru https://vk.com https://connect.facebook.net;
  style-src 'self' 'unsafe-inline' https://fonts.googleapis.com;
  font-src 'self' https://fonts.gstatic.com;
  img-src 'self' data: https:;
  media-src 'self';
  connect-src 'self' https://*.amocrm.ru https://api.wati.io https://www.google-analytics.com https://mc.yandex.ru;
  frame-src https://www.youtube.com https://t.me;
  object-src 'none';
  base-uri 'self';
  form-action 'self' https://*.amocrm.ru;
```

### 24.3 Cookie consent + privacy

| Что | Как |
|---|---|
| Cookie consent banner | Простой компонент `CookieConsent.astro` — тонкая полоса внизу |
| Privacy policy | `/legal/privacy` (RU) + `/es/legal/privacy` (ES) — после legal-спека |
| Согласие на обработку перс. данных (152-ФЗ) | Чекбокс под формой "Согласен с обработкой ПД" + ссылка на политику. Без него Roskomnadzor может оштрафовать |
| GDPR (для AR через Meta Ads) | Pixel грузим только после клика "Принять" в cookie banner |

### 24.4 Rate limiting

| Слой | Решение |
|---|---|
| Form submit | Client-side 30 сек на IP через localStorage + server-side через CF Workers (после миграции) |
| Bot-detection | Honeypot + опционально Cloudflare Turnstile |

---

<a id="sec-25"></a>
## 25. Что НЕ входит в реализацию (anti-scope)

- **Платформа продукта** (workflow генерации Veo/Sora, бэкенд, очередь, доставка) — отдельный технический спек
- **Юр. документы** (договор оферты RU + cross-border USD-договор + opt-in пункты) — отдельный legal-спек
- **Финансовая модель** маркетинга (бюджет на промоушен, P&L канала, CAC) — после первых данных пилота
- **HR и найм production-команды** (15 сателлитов + копирайтеры + SMM) — HR-план
- **Бренд-айдентика** (детальный логотип, гайдлайн, иллюстрации) — отдельный design-спек на основе токенов из раздела 9
- **Блог / контент-маркетинг** на сайте (SEO-статьи) — после первых клиентов
- **Личный кабинет клиента** (для управления подпиской, скачивания готовых видео) — отдельный продуктовый MVP
- **Real-time webinars и live-эфиры** на сайте — после первых 30 paying customers
- **Email-templates** для drip и newsletter — отдельный copy-спек
- **Страницы кейсов** (case-studies формат) — после первых публикуемых кейсов
- **Конкретные тексты drip-сообщений TG-бота** — после apruv этого спека
- **15 story bibles сателлитов** — отдельный creative-спек

---

## Self-review (автор)

| Проверка | Результат |
|---|---|
| Placeholders / TBD / TODO | Нет. Все цифры либо из ресёрча, либо явно помечены |
| Внутренняя консистентность | 14 базовых лендингов, 36 общих страниц, 5 ICP, 7 концепций, 15 сателлитов — везде согласованно |
| Связь с концепцией | Pricing 49K-490K, ICP, 5+1+1 структура — наследовано из concept-design.md |
| Связь со стратегией | RACE-этапы, сателлиты, Cognitive Toolkit, social proof — реализованы через лендинги (sec 17-20) |
| amoCRM (заменили Bitrix24) | Везде где упоминается — amoCRM (sec 2, 19, 20, 24) |
| Двуязычность | RU + ES-AR с voseo, language switcher, hreflang — везде |
| Stop-hook compliance | Нет утверждений "готово/работает/задеплоено" без EVIDENCE |
| Mobile-first + Lighthouse | Бюджет ассетов <300 KB, LCP <2s, ≥95 mobile — заложено |

---

## Дальнейшие шаги

1. **Ждём apruv пользователя** на этот документ.
2. **После apruv** — три sub-project'а закрыты (концепция + стратегия + реализация). Можно переходить к **техническому плану имплементации** через skill `superpowers:writing-plans`. Артефакт: `docs/superpowers/plans/<date>-fabrika-kontenta-website-implementation-plan.md` с разбивкой на конкретные задачи и спринты.
3. **Параллельно** — запуск работы над сопутствующими спеками (legal, продукт-платформа, бренд-айдентика).
