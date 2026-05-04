# Фабрика Контента — документация проекта

Полный набор design-документов: ресёрч, концепция, маркетинг-стратегия, реализация сайта, план разработки.

**Live preview сайта:** https://ssardcompany-web.github.io/

## Структура

### 1. Ресёрч (9 документов)

Сырые данные рынка — 161+ болей с цитатами, 90+ компаний, 130+ источников РФ + Аргентина.

- [Главный сводный документ](./research/2026-05-02-fabrika-kontenta-research.md)
- Подробные partials:
  - [01 · RU-конкуренты в нише AI/контент](./research/_partials/01-ru-competitors.md)
  - [02 · Глобальные AI-video SaaS + конверсионные паттерны](./research/_partials/02-global-ai-video-saas.md)
  - [03 · Боли селлеров WB/Ozon одежды](./research/_partials/03-pains-marketplace-sellers.md)
  - [04 · Боли DTC fashion-брендов и ателье](./research/_partials/04-pains-dtc-and-ateliers.md)
  - [05 · Боли швейных фабрик B2B + крупных fashion-брендов](./research/_partials/05-pains-factories-and-big-brands.md)
  - [06 · Аргентина — рынок и конкуренты](./research/_partials/06-argentina-market.md)
  - [07 · Аргентина — боли по 5 сегментам](./research/_partials/07-argentina-pains.md)
  - [08 · Реальные тарифы AI-видео для одежды (РФ + AR + глобал)](./research/_partials/08-pricing-deep-dive.md)

### 2. Дизайн-спецификации (3 документа)

- [Концепция продукта](./specs/2026-05-03-fabrika-kontenta-concept-design.md) — 5 ICP, value proposition × сегмент × рынок, Pricing-Гибрид, локализация, 7 концепций лендингов
- [Маркетинговая стратегия](./specs/2026-05-03-fabrika-kontenta-marketing-strategy.md) — RACE-фреймворк, 15 stealth-сателлитов, Cognitive Toolkit (12 biases), 4-дневный TG-drip, multi-channel nurture, Runner 90 дней
- [Реализация сайта](./specs/2026-05-03-fabrika-kontenta-website-implementation.md) — Astro+Tailwind архитектура single-site/multi-variant, дизайн-система, копирайт L1 RU+ES, amoCRM/Wati/TG-bot, аналитика, performance, SEO, безопасность

### 3. Implementation план (1 документ)

- [Технический план реализации сайта](./plans/2026-05-03-fabrika-kontenta-website-implementation-plan.md) — 53 задачи в 5 фазах, bite-sized шаги с конкретным кодом и командами

### 4. Документация самого сайта (2 документа)

- [README сайта](./site/site-README.md)
- [Operations Runbook](./site/site-runbook.md) — инструкция publish/rollback/edit

## Ссылки на live-preview сайта

Публичный preview всех 36 страниц для команды:

**Базовые RU:**
- https://ssardcompany-web.github.io/
- https://ssardcompany-web.github.io/marketplace-sellers/
- https://ssardcompany-web.github.io/dtc-fashion/
- https://ssardcompany-web.github.io/atelier/
- https://ssardcompany-web.github.io/fabrika/
- https://ssardcompany-web.github.io/enterprise/

**A/B-варианты RU:**
- `/v/<page>/b/` и `/v/<page>/c/` для каждого из 6 сегментов (12 URL)

**ES-зеркало:** `/es/...` и `/es/v/<page>/<b|c>/`

## Стек проекта

- Astro 6 SSG + Tailwind 4 + TypeScript strict
- Content Collections + zod schema (single source of truth для variants)
- amoCRM (5 воронок ICP) + Wati (WhatsApp Business AR) + Salebot (Telegram-бот)
- GA4 + Yandex.Metrika + Pixels (Yandex/Meta/VK) + Looker Studio дашборд
- Lighthouse target ≥95 mobile, LCP <2.5s, bundle <300KB

## Сборка статистики

| Группа | Документов | Объём |
|---|---|---|
| Ресёрч | 9 | ~390 KB |
| Дизайн-спеки | 3 | ~183 KB |
| План реализации | 1 | ~117 KB |
| Документация сайта | 2 | ~12 KB |
| **Итого** | **15** | **~702 KB** |

## Лицензия

Внутренняя документация. Все права защищены.
