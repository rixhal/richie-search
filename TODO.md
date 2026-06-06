# Richie Search — Ausbau-Roadmap (Frontend + Backend)

> Custom Dark-Theme Suchmaschine. search.richie.fyi.
> Stand: Juni 2026. Single-File HTML, SearXNG Docker, NGINX.

---

## 🔴 Phase 1 — Search Quality First

- [ ] **News-Sortierung** — `publishedDate`-basierte Sortierung (neueste zuerst) im Frontend. Ohne Datum → ans Ende. `time_range`-Filter in SearXNG fixen.
- [ ] **Google News aktivieren** — `google_news` Engine in `settings.yml` (liefert konsistentere Ergebnisse als bing news)
- [ ] **Pagination** — Seiten-Navigation oder „Mehr laden"-Button. SearXNG `pageno` Parameter.
- [ ] **Leere Medien-Fallback** — Bilder/Videos ohne Thumbnail zeigen Platzhalter, nicht einfach nichts.
- [ ] **Timeout + Retry** — `AbortController` mit 10s Limit + „Erneut versuchen".
- [ ] **URL-State-Fix** — `time_range` in Browser-URL syncen (Back-Button-restore).

---

## 🟡 Phase 2 — UX & Alltag

- [ ] **Favicon + PWA-Manifest** — Eigenes Icon (rotes R), `manifest.json`.
- [ ] **Dark/Light Toggle** — `data-theme` togglen, `localStorage` persist.
- [ ] **Suchverlauf** — Letzte 10 Queries in `localStorage`, Dropdown bei Fokus.
- [ ] **„Link kopieren" pro Result** — Teilen-Button, URL in Zwischenablage.
- [ ] **Safesearch-Toggle** — Dropdown: Aus/Moderat/Strikt → `safesearch=0|1|2`.
- [ ] **Sprach-Filter** — Dropdown: `de|en|auto`. Aktuell hardcoded `de`.
- [ ] **Responsive Grid** — Mobile: Bilder 2-spaltig, Video-Karten gleichmäßig.

---

## 🟢 Phase 3 — Productivity Features

- [ ] **Bang-Search** — `!yt Suchbegriff` → YouTube, `!gh` → GitHub, `!wiki` → Wikipedia, `!ddg` → DuckDuckGo, `!px` → Perplexity. Client-seitiger Redirect.
- [ ] **Instant Answers** — SearXNG `answers[]` + `infoboxes[]` als Hero-Card rendern (Wiki-Infobox, Calculator, Unit-Converter).
- [ ] **Research Collections** — Ergebnisse in benannte Sammlungen speichern (localStorage JSON). Export als Markdown.
- [ ] **Diff-Search-Indikator** — Bei wiederholten Queries: „3 neue Ergebnisse seit gestern"-Badge.
- [ ] **Keyboard-Nav** — `j`/`k`/`Enter` in Result-Cards, `/` für Suchfeld-Fokus.

---

## 🔵 Phase 4 — Backend-Tuning

- [ ] **Engine-Optimierung** — Nur relevante Engines aktiv. `google`, `google_news`, `brave`, `ddg`, `wikipedia`, `github`, `stackoverflow`, `reddit`. Rest deaktiviert = schneller.
- [ ] **NGINX-Caching** — `proxy_cache` für API-Responses (30s–5min TTL je nach Kategorie).
- [ ] **Source Trust Tiers** — ArXiv/Wikipedia grün, Reddit/Social gelb, Sonstige grau. CSS-Indikator pro Card.
- [ ] **Export-Tools** — Ergebnisse als JSON/CSV/OPML + „In Obsidian speichern"-Button.

---

## ⚫ Phase 5 — Research Engine (Cron/Hermes)

> Diese Features leben NICHT im Web-Frontend, sondern als Hermes-Skills/Crons.
> Siehe [RESEARCH.md](RESEARCH.md).

| Konzept | Beschreibung |
|----------|-------------|
| Self-Improving Search Loop | Seed-Query → Entitäten extrahieren → 3 neue Queries → iterieren → Dossier |
| Knowledge Graph | Entitäten + Beziehungen aus Suchergebnissen über Zeit aufbauen |
| Adversarial Search | Gezielte Gegen-Queries, Gegenargumente synthetisieren |
| Emergence Detection | Semantische Cluster-Erkennung über unabhängige Quellen |
| Research Replay | Session-Aufzeichnung: Queries, Pfade, Erkenntnisse, Diffs |
| Predictive Research | Trend-Vorhersage aus Publikations-Frequenz + Cross-References |
| LLM Summary | Hermes fasst Top-5 URLs zusammen (spart Perplexity-Tokens) |
| Perplexity Deep-Dive | Query + Quell-URLs automatisch an Perplexity weiterleiten |

---

## 📋 Done

- [x] Rotes Brand-Theme (Custom CSS + SearXNG-Variablen)
- [x] SearXNG Docker Backend + Valkey
- [x] NGINX Reverse Proxy mit API-Rewrite
- [x] Cloudflare Tunnel
- [x] Kategorie-Tabs (10 Kategorien)
- [x] Zeitfilter
- [x] Autocomplete
- [x] GitHub Repo (rixhal/richie-search)
- [x] Mobile-Responsive Basis
- [x] Bilder-Grid + Video-Karten
- [x] Google + Bing + Brave + Startpage + DuckDuckGo Engines

---

## 🔧 Dev-Notizen

- **Deploy:** `sudo cp /tmp/... /var/www/search.richie.fyi/` → `sudo nginx -s reload`
- **Backups:** `.bak.YYYYMMDD-HHMMSS`, per `.gitignore` ausgeschlossen
- **CF-Cache:** `?v=$(date +%s)` zum Cache-Busting, Purge via API
- **SearXNG:** `2026.5.26`, Docker `searxng-privacy:latest`
- **Kein Build-Step:** Single-File HTML, kein npm/Bundler/Framework
