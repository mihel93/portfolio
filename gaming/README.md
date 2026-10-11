# Gaming Industry Market Benchmark 2026

> 100,000 simulated game-month records · 2,400 titles · 11 genres · 6 platforms · dual Parquet/CSV · chart gallery included · **CC BY-SA 4.0**

A relational benchmark for **gaming-industry analysts and product managers**: monthly performance records for 2,400 fictional games — MAU, peak concurrent users, paying share, revenue, review scores, streamer hours and 30-day retention across genres, platforms and monetization models.

**Note**: game titles are fictional — no real studio data is represented. Genre/platform/market-share reference rows are public industry knowledge.

## Multi-table relational architecture

| Table | Rows | What it holds |
|---|---|---|
| `data/game_performance` | **100,000 game-months** (15 cols) | One row per game-month: title × genre × platform × monetization → MAU, peak CCU, paying share, revenue, review score, streamer hours, retention, season |
| `data/genres` | 11 genres | Public genre descriptions, revenue shares, typical MAU scale |
| `data/platforms` | 6 platforms | PC/Steam, PlayStation, Xbox, Switch, iOS, Android — market shares |
| `data/monetization_models` | 4 models | premium, F2P, subscription, early-access — ARPPU anchors |
| `data/genre_profile` | 11 rows | Pre-aggregated genre profile: MAU, CCU, revenue, reviews, retention |

**Every table ships twice**: Snappy Parquet + plain CSV.

## Chart gallery (`images/`) — dark theme, 150 DPI

1. `01_revenue_by_genre.png` — revenue by genre (live-service dominance)
2. `02_mau_by_platform.png` — player base by platform
3. `03_reviews_by_monetization.png` — reviews by monetization model
4. `04_mau_vs_revenue.png` — audience vs revenue (paying-share colored)
5. `05_seasonality.png` — holiday-quarter seasonality
6. `06_retention_by_genre.png` — retention by genre
7. `07_platform_streams.png` — platform mix + streamer halo

`cover.jpg` is the 16:9 cover.

## Quickstart

```python
import pandas as pd

t = pd.read_parquet("data/game_performance.parquet")

# F2P vs premium revenue per MAU
t["rev_per_mau"] = t.revenue_k_usd * 1000 / (t.monthly_active_users_k * 1000)
t.groupby("monetization").rev_per_mau.median().round(3)
```

## Methodology & provenance (please read)

- **Game-month rows are simulated** (2019–2026, seed `20261018`). Documented generator: MAU anchored to genre MAU scale × lognormal; paying share beta-distributed (F2P low, premium = 100%); revenue = MAU × paying × ARPPU (F2P) or premium units; holiday quarter +35%; retention declines with game age.
- **Reference tables hold public knowledge** (genres, platforms, monetization); no real sales or player counts.
- Sanity anchors: holiday months peak; review scores slightly favor premium titles.

## Inspiration & research use cases

- **Live-ops analytics**: retention curves and MAU decay by genre.
- **Monetization studies**: ARPPU vs review-score tension.
- **Market sizing**: platform shares and genre revenue mix.
- **Streamer-effect analysis**: streaming hours vs MAU growth.
- Teach **entertainment analytics**: simple enough to lecture from.

## License & attribution

**CC BY-SA 4.0** — credit *dianatofficial / Gaming Industry Market Benchmark 2026*, share adaptations alike. Platform names are trademarks of their owners; all game titles are fictional.

## Version notes

- **v1.0 — 2026-10-11** — Initial release: 100K game-month records, 4 reference tables, genre profile, 7-chart gallery, cover.
