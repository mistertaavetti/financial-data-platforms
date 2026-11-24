# Financial Data Platforms

> Case study: three financial-data platforms — a corporate index, a dividend tracker, and a deal-flow dashboard — built on one shared Next.js + Python stack at a national business newsroom.
> Built for **Nairametrics — Nigeria's largest business newsroom**.
> **Code is proprietary; this repo documents the build** — architecture, decisions, and outcomes.

---

## Context

| | |
|---|---|
| **Client** | Nairametrics |
| **Platforms** | Corporate index & ranking · dividend tracking · deal-flow management |
| **Role** | Full-stack — frontend, backend API, data pipelines |
| **Timeline** | 2024 — present (ongoing platforms) |

## Problem

Financial data products are a natural extension of a business newsroom's audience, but each one is its own platform problem: rankings that must be reproducible, dividend histories that must be accurate, deal pipelines that must be searchable — all SEO-critical, all feeding the same commercial audience, and all maintained by a small engineering team. The answer was not three bespoke stacks but one shared architecture, specialized per product.

## The Shared Stack

All three platforms share the same shape:

| Layer | Technology |
|---|---|
| Frontend | Next.js (14/15), TypeScript, Tailwind CSS — SEO-first (SSR, structured data, sitemap, OG images) |
| Backend | Python (Django 3.2 / DRF on the index; Python APIs on the others) |
| Database | PostgreSQL |
| Data ingestion | pandas + CSV parsing for bulk uploads |
| Auth | JWT (Django simplejwt / token APIs), email verification |

Building the second and third platforms on the first one's boilerplate — rather than starting fresh — is the central engineering story here: shared SEO machinery, shared component patterns, shared deployment shape, with data models and visualizations specialized per product.

## Platform 1 — Corporate Index & Ranking

A corporate ranking and financial-data platform: a sortable, searchable, filterable directory of Nigeria's top companies with deep profile pages.

- **Company profiles** with dynamic tabs: financial performance, directors, dividends, capital structure, competitive analysis, CSR, ratios, tax, top shareholders
- **Side-by-side comparison** of up to 4 companies
- **Interactive visualizations** (ApexCharts, ECharts, Chart.js) for trends and ratios
- **Admin CSV upload pipeline** for bulk company data, processed with pandas
- **Multi-currency support** (NGN / USD / GBP / EUR)
- Backend: Django 3.2 + Django REST Framework 3.13 with Swagger API docs

### The bought-theme lesson

The index platform began on a purchased admin theme. It shipped fast, but the theme became a hard ceiling — SEO couldn't match the newsroom's other properties, and deep changes meant fighting the theme's assumptions. I documented an 8-phase migration plan (with ADRs and a handoff package) to port the platform onto the shared, owned stack: keep the data visualizations, replace everything the theme controlled. That plan now guides the rebuild — and it's why platforms two and three were built on owned boilerplate from day one.

## Platform 2 — Dividend Tracking

Dividend tracking and analysis for an investor audience.

- Per-company dividend histories, yield and payout-ratio calculations
- Dividend history visualization
- Rich-text content management (Draft.js / SunEditor) and file upload (React Dropzone) for editorial content around the data
- Next.js 15 + React 19 frontend on the shared stack

## Platform 3 — Deal-Flow Dashboard

Deal-flow management for tracking transactions and deals.

- Deal pipeline tracking and detail pages with financial metrics
- Search and filter across deals
- SEO module ported directly from the shared boilerplate — closing the gap with the other properties took days, not months, because the machinery was shared

## Key Technical Decisions

| Decision | Rationale | Trade-off |
|---|---|---|
| One owned boilerplate across platforms | Three products maintained by a small team; every fix and SEO improvement lands everywhere | Boilerplate must be genuinely generic — product-specific hacks in shared code multiply fast |
| SEO-first architecture (SSR, structured data, ISR) | Search is the acquisition channel for a newsroom's data products | More engineering discipline than a client-rendered dashboard |
| pandas + CSV for data ingestion | Newsroom analysts work in spreadsheets; bulk upload keeps them unblocked | Robust CSV validation and schema versioning required |
| Django/DRF where a full framework pays off | Admin, auth, and ORM maturity for the most data-heavy platform | Heavier than FastAPI for the lighter services |
| 8-phase migration plan with ADRs instead of a stealth rewrite | A rewrite that big needs reviewable, reversible steps and a handoff another engineer can execute | Planning overhead — but the alternative is an unbounded rewrite |

## Outcomes

- Three financial-data platforms serving the newsroom's investor audience — stock recommendations, corporate rankings, dividend tracking, and deal flow — operated as one coherent product family on one stack.
- Shared SEO architecture with server-side rendering and structured data; **90+ Lighthouse scores reported** across the properties.
- The bought-theme migration is planned as reviewable ADRs and phases rather than a risky big-bang rewrite.

## What I'd Do Differently

- **Start on owned boilerplate everywhere.** The bought theme bought speed and sold ceilings; the lesson (documented in the migration ADRs) is now part of how new platforms get greenlit.
- **Shared component library from the start.** The three frontends share patterns informally; an explicit library would have removed more duplication.
- **Backend test coverage earlier.** Frontend tests were prioritized; the Python APIs deserve the same discipline, and it's on the roadmap.

## Evidence

- **Live platforms:** available on request.
- **Screenshots:** available on request.

---

*Case study by David Aloba. [mistertaavetti.davidaloba.com](https://mistertaavetti.davidaloba.com) · [LinkedIn](https://linkedin.com/in/mistertaavetti)*
