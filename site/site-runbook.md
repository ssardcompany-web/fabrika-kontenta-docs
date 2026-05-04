# Operations Runbook — Фабрика Контента

## Локальная разработка

```bash
npm install
cp .env.example .env.local   # заполнить amoCRM/аналитика ID
npm run dev                  # http://localhost:4321
```

Доступные команды:

```bash
npm run dev           # dev-сервер с HMR
npm run build         # static export → dist/
npm run preview       # просмотр собранной dist локально
npm test              # vitest unit
npm run check         # astro check (TS + content schema)
npm run lighthouse-ci # Lighthouse аудит на dist/
npm run linkcheck     # все внутренние ссылки живы
npm run a11y          # axe-core (требует запущенный dev на :4322)
```

## Публикация

### Первый раз
1. Скопировать `.env.deploy.example` в `.env.deploy`
2. Заполнить `BEGET_USER`, `BEGET_HOST`, `BEGET_PATH`
3. Сгенерировать SSH-ключ: `ssh-keygen -t ed25519 -C "deploy@fabrika-kontenta.ru"`
4. Добавить публичный ключ в beget admin → SSH-ключи
5. Тест: `ssh user@beget "echo OK"`

### Каждая публикация
```bash
./scripts/publish.sh
```

Скрипт:
1. Build → dist/
2. Linkcheck (если broken — стоп)
3. Backup текущей версии на сервере → `~/backups/site-YYYYMMDD-HHMMSS.tar.gz`
4. rsync новой версии на beget
5. Smoke-test 8 ключевых URL

### Тест-запуск без реальной публикации
```bash
DRY_RUN=1 ./scripts/publish.sh
```

### Откат
```bash
./scripts/rollback.sh site-20260503-160000.tar.gz
```

## Как редактировать копирайт

JSON-файлы в `src/data/pages/`:
- Имя: `<type>-<variant>-<lang>.json` (всё lowercase, разделитель — дефис)
- Пример: `marketplace-sellers-A-ru.json`
- При сохранении Astro slugify lowercase'ит имя — id в коде = `marketplace-sellers-a-ru`

После правки:
```bash
npm run check     # валидация zod schema
npm run build     # сборка
npm run preview   # визуальный просмотр
./scripts/publish.sh
```

## Как добавить вариант D (новый A/B-тест на сегмент)

Пример: добавить вариант D к маркетплейс-селлерам RU.

1. Скопировать дефолт:
   ```bash
   cp src/data/pages/marketplace-sellers-A-ru.json src/data/pages/marketplace-sellers-D-ru.json
   ```
2. Изменить `meta.variant` на `"D"`, добавить `"noindex": true`, оставить тот же `meta.canonical` (ведёт на A для SEO)
3. Изменить ключевые поля (Hero / CTA / pricing-якорь)
4. Добавить `'d'` в массив `VARIANTS` в `src/pages/v/[page]/[variant].astro`
5. Build + публикация
6. URL: `/v/marketplace-sellers/d/`

## Как добавить новый тип страницы (например L6 — корпоративные униформы)

1. Добавить `'corporate-uniforms'` в enum `meta.type` в `src/content.config.ts`
2. Создать `src/data/pages/corporate-uniforms-A-ru.json` + `corporate-uniforms-A-es.json`
3. Создать роуты:
   - `src/pages/corporate-uniforms.astro` (RU)
   - `src/pages/es/uniformes.astro` (или другой ES-slug)
4. Добавить `'corporate-uniforms'` в массив `PAGES` в обоих `[variant].astro` для variants
5. Build + публикация

## Архитектура

- **Astro 6** SSG → `dist/` → rsync на beget (статика, без Node.js на сервере)
- **Tailwind 4** с дизайн-токенами в `src/styles/global.css`
- **Content Collections** + zod схема в `src/content.config.ts`
- **i18n**: RU дефолт без префикса, ES с `/es/`
- **A/B/C** через UTM на разные URL: `/v/<page>/<b|c>/`

## Аналитика и интеграции

- amoCRM webhook через `src/lib/amocrm.ts` (ENV переменные `PUBLIC_AMOCRM_*`)
- GA4 + Yandex.Metrika + Pixels — lazy-load после `requestIdleCallback` через `src/lib/analytics.ts`
- Cookie-consent — Pixel'ы загружаются только после accept
- Trail форм — UTM сохраняются в `localStorage` на 30 дней + передаются в hidden-поля + в amoCRM custom-fields
- TG-бот redirect: после submit формы — переход на `t.me/fabrika_contenta_bot?start=<segment>_<variant>_<utm_campaign>_<utm_content>`

## Где смотреть метрики

- GA4 — основной conversion funnel
- Yandex.Metrika — webvisor + heatmaps
- amoCRM — pipeline по сегментам, UTM в карточке лида
- Looker Studio дашборд — RACE-метрики (см. отдельный URL после настройки)

## Что НЕ работает без `.env.local`

- Submit формы → amoCRM (есть fallback в `localStorage.fabrika_lead_queue`, который потом импортируется руками)
- GA4 / Метрика / Pixels (тихо не загружаются)

Это **намеренно** — позволяет крутить сайт локально без реальных интеграций.
