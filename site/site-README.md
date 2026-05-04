# Фабрика Контента — сайт

Single-site с multi-variant архитектурой: 6 типов лендингов × 3 варианта (A/B/C) × 2 языка (RU + ES-AR с voseo) = **36 страниц** на одной кодовой базе.

**Stack:** Astro 6 SSG + Tailwind 4 + TypeScript strict + amoCRM + Wati WhatsApp + Telegram-бот + GA4/Yandex.Metrika/Pixels.

## Быстрый старт

```bash
npm install
cp .env.example .env.local        # заполнить amoCRM/аналитика ID (опционально для локальной разработки)
npm run dev                       # http://localhost:4321
```

## Команды

| Команда | Что делает |
|---|---|
| `npm run dev` | Dev-сервер с HMR на :4321 |
| `npm run build` | Static export → `dist/` |
| `npm run preview` | Просмотр собранной dist локально |
| `npm test` | Vitest unit (utm, lib) |
| `npm run check` | astro check (TS + content schema) |
| `npm run lighthouse-ci` | Lighthouse аудит (требует Chrome) |
| `npm run linkcheck` | Проверка broken-ссылок в dist/ |
| `npm run a11y` | axe-core WCAG 2.1 AA (требует dev-сервер) |

## Структура

```
src/
├── components/       # 12 переиспользуемых блоков (Hero/Pain/Solution/Pricing/FAQ/Form/...)
│   └── ui/           # Button/Tag/Card/Badge UI-примитивы
├── content.config.ts # zod schema для страничных variants
├── data/pages/       # 36 JSON: <type>-<A|B|C>-<ru|es>.json
├── layouts/          # BaseLayout (head/meta) + LandingLayout (composition)
├── lib/              # utm.ts, form-submit.ts, amocrm.ts, analytics.ts, track-events.ts
├── pages/            # 12 base routes RU/ES + dynamic /v/[page]/[variant]
└── styles/global.css # Tailwind 4 design tokens

public/
├── .htaccess         # CSP + cache headers + force-HTTPS
├── fonts/            # Cormorant Garamond + Inter + Italiana woff2
├── og/               # OG-карточки 1200×630 (генерируется отдельно)
└── videos/           # Hero-видео + demo-карусель
```

## Документация

- **Operations Runbook:** [`docs/runbook.md`](./docs/runbook.md)
- **Полная дизайн-спека:** `../ＮＥＫＴＯ/docs/superpowers/specs/2026-05-03-fabrika-kontenta-website-implementation.md`
- **План реализации:** `../ＮＥＫＴＯ/docs/superpowers/plans/2026-05-03-fabrika-kontenta-website-implementation-plan.md`

## Публикация на beget

```bash
cp .env.deploy.example .env.deploy
# заполнить BEGET_USER, BEGET_HOST, BEGET_PATH

./scripts/publish.sh           # build + linkcheck + backup + rsync + smoke-test
./scripts/rollback.sh <name>   # откат на бэкап
```

## A/B/C-тесты

```
/marketplace-sellers/        ← вариант A (default, индексируется)
/v/marketplace-sellers/b/    ← вариант B (noindex, для UTM-трафика)
/v/marketplace-sellers/c/    ← вариант C (noindex)
```

UTM сохраняется в localStorage (TTL 30 дней) → передаётся в hidden-поля формы → попадает в amoCRM custom-fields. Победителя теста определяем в GA4 по разрезу `variant` × `lead_segment`.
