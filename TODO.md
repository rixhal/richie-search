# Richie Search — Ausbau-Roadmap

> Custom Dark-Theme Suchmaschine auf SearXNG. Stand: Juni 2026.
> Single-File HTML, DM Sans + JetBrains Mono, rotes Brand-Theme.

---

## 🔴 Phase 1 — Bugfixes & Stabilität

- [ ] **Seitennavigation (Pagination)** — aktuell nur erste Seite. SearXNG gibt `pageno`/`next` im JSON. Buttons: „Mehr laden" oder Seitenzahlen.
- [ ] **Fehler bei leeren Bild-/Video-Ergebnissen** — `renderImages`/`renderVideos` zeigen kein Empty-State wenn `img_src`/`thumbnail` fehlt. Fallback oder „Keine Medien" anzeigen.
- [ ] **Such-Timeout & Retry** — `doSearch()` hat kein Timeout. `AbortController` + 10s Limit + „Erneut versuchen"-Button.
- [ ] **URL-State robust** — `time_range` fehlt im URL-Parameter (wird nicht restored). Query-Parameter mit Time-Range syncen.

---

## 🟡 Phase 2 — UX & Komfort

- [ ] **Favicon + Manifest** — eigenes Icon (rotes R auf Schwarz) statt SearXNG-Favicon. `manifest.json` für PWA.
- [ ] **Dark/Light Toggle** — CSS-Variablen sind schon da. Button in Footer/Header der `data-theme` toggled. In `localStorage` speichern.
- [ ] **Suchverlauf** — letzte 10 Suchen in `localStorage`. Dropdown beim Fokussieren des leeren Suchfelds.
- [ ] **Keyboard-Navigation in Ergebnissen** — Tab/Arrow durch Result-Cards, Enter zum Öffnen. `j`/`k` wie bei Vimium.
- [ ] **Responsive Grid-Polish** — Bilder-Kacheln auf kleinen Screens 2-spaltig statt 1. Video-Cards Höhe normalisieren.
- [ ] **Lazy Load Thumbnails** — aktuell lädt `loading="lazy"` erst ab Bild 12. Alle Bilder außer den ersten 4 auf `lazy`.

---

## 🟢 Phase 3 — Features

- [ ] **Custom Search Shortcuts** — `!yt Suchbegriff` → YouTube-Suche, `!gh` → GitHub, `!wiki` → Wikipedia, `!ddg` → DuckDuckGo. Bang-Syntax parsen + Redirect.
- [ ] **Safesearch-Toggle** — Button neben Zeitfilter: Aus/Moderat/Strikt → `safesearch=0|1|2` Parameter.
- [ ] **Ergebnis-Teilen** — „Link kopieren"-Button pro Result-Card. URL in Zwischenablage mit Titel.
- [ ] **Instant Answers** — SearXNG liefert `answers[]` und `infoboxes[]`. Oben als Card rendern (wie Google Knowledge Panel).
- [ ] **Sprach-Filter** — Dropdown neben Zeitfilter: `language=de|en|auto`. Aktuell hardcoded `de`.

---

## 🔵 Phase 4 — Backend & Engine-Tuning

- [ ] **SearXNG `settings.yml` optimieren** — Nur relevante Engines aktivieren (Google, Brave, DDG, Wikipedia, Reddit, GitHub, StackOverflow, etc.). Deaktivierte Engines = schneller.
- [ ] **Eigene Engines hinzufügen** — `searxng_data` checken ob SearXNG Custom-Engine-Support hat. Z.B. interne Dienste oder Nischen-Suchmaschinen.
- [ ] **Caching-Layer** — NGINX `proxy_cache` für SearXNG-API-Responses (5min TTL). Reduziert Last bei Mehrfach-Suchen.
- [ ] **Healthcheck-Endpoint** — `/api/health` in NGINX, der SearXNG `/healthz` anpingt. Für Monitoring/Cron.

---

## ⚫ Phase 5 — nice to have

- [ ] **Audio Search** — SearXNG supported Audio-Kategorie? Prüfen. UI: Mini-Player in Result-Cards.
- [ ] **Torrent/Magnet-Ergebnisse** — Files-Kategorie checken. Magnet-Links als Button rendern.
- [ ] **Export-Funktion** — Ergebnisse als JSON/CSV/OPML exportieren. Button in Footer.
- [ ] **Browser-Such-Plug-in** — OpenSearch-Description anpassen (ist schon da, aber auf SearXNG-Default). Eigenes Icon, eigener Name.
- [ ] **Mehrsprachiges UI** — `data-lang` Attribute. Deutsch/Englisch. `navigator.language` Auto-Detect.

---

## 📋 Done

- [x] Rotes Brand-Theme (Custom CSS + SearXNG CSS-Variablen)
- [x] SearXNG Docker Backend
- [x] NGINX Reverse Proxy mit API-Rewrite
- [x] Cloudflare Tunnel (search.richie.fyi)
- [x] Kategorie-Tabs (10 Kategorien)
- [x] Zeitfilter (24h/Woche/Monat/Jahr)
- [x] Autocomplete
- [x] GitHub Repo (rixhal/richie-search)
- [x] Mobile-Responsive Basis-Layout
- [x] Bilder-Grid + Video-Karten Rendering

---

## 🔧 Dev-Notizen

- **Deployment:** `sudo cp /tmp/... /var/www/search.richie.fyi/` → `sudo nginx -s reload`
- **Backups:** `.bak.YYYYMMDD-HHMMSS` im selben Verzeichnis, per `.gitignore` ausgeschlossen
- **CF-Cache:** Cloudflare cached statische Assets. Nach Deploy: `?v=$(date +%s)` zum Testen, Purge via API
- **SearXNG Version:** `2026.5.26` (Docker: `searxng-privacy:latest`)
- **Kein Build-Step:** Alles Single-File. Kein npm, kein Bundler, kein Framework.
