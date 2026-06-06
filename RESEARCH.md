# Research Engine — Hermes/Cron Features

> Autarke Recherche-Infrastruktur auf Basis SearXNG + Hermes.
> Diese Features laufen als Skills/Crons, NICHT im Web-Frontend.
> Ergebnisse per Telegram oder in Obsidian-Vault.

---

## 🔄 Self-Improving Search Loop

**Konzept:** Eine Query startet einen Recherche-Thread, der sich selbst iterativ vertieft.

```
Seed-Query → SearXNG (Top 10 URLs)
  → Hermes: Entitäten + Fakten extrahieren
    → 3 neue Queries generieren (Spezifizierung, Gegenposition, Nachbarfeld)
      → Nächste Runde SearXNG
        → Nach 3 Iterationen: Dossier mit Quellen, Widersprüchen, blinden Flecken
```

**Output:** Markdown-Dossier mit Quellenverzeichnis, Fakten, offenen Fragen. Per Telegram zugestellt.

**Trigger:** Cron (täglich/wöchentlich) oder manuell per Telegram-Command `/research <topic>`.

---

## 🧠 Knowledge Graph

**Konzept:** Jede Suche baut einen persönlichen Wissensgraph auf.

- Entitäten: Personen, Firmen, Technologien, Projekte
- Beziehungen: „arbeitet an", „konkurriert mit", „zitiert", „widerspricht"
- Timelines: Wann tauchte eine Entität zuerst auf? Frequenz über Zeit?

**Technik:** Hermes extrahiert Entitäten aus Search-Ergebnissen → speichert in JSON-Graph → bei neuen Suchen automatisch anreichern.

**Output:** Graph-Visualisierung als SVG, Query-Interface: „Was weiß ich über X?"

---

## ⚔️ Adversarial Search

**Konzept:** Nicht „was sagt das Internet zu X", sondern: Suche EXPLIZIT nach Widerspruch.

```
Query: "Ist Technologie X sicher?"
  → Hermes formuliert Gegen-Queries:
    "Technologie X Sicherheitslücken 2026"
    "Kritik an Technologie X"
    "Technologie X FAIL"
  → SearXNG sucht Gegen-Stimmen
  → Perplexity synthetisiert stärkste Gegenargumente
```

**Output:** Pro/Contra-Dossier, gewichtet nach Quellen-Qualität. Kein Bestätigungs-Bias.

---

## 📡 Emergence Detection

**Konzept:** Semantische Cluster-Erkennung — nicht Keyword-Monitoring.

Statt „enthält dieser Artikel das Wort X?" fragt das System:
„Diskutieren 4+ Quellen aus unabhängigen Domains plötzlich dasselbe Konzept — auch mit unterschiedlichen Worten?"

**Technik:** Embedding-basiertes Clustering (Hermes calls embedding API). Topics die Fahrt aufnehmen → Alert.

**Output:** Telegram-Alert: „3 Quellen diskutieren seit gestern Konzept Y — Trend?"

---

## 🎬 Research Replay (Git für Gedanken)

**Konzept:** Jede Recherche-Session aufzeichnen: Queries, geklickte Links, Erkenntnisse, Sackgassen.

Später: „Zeig mir meine Recherche zu BLE HCI vom Mai 2026" → Timeline mit Diff zu heute.

**Technik:** Session-Log als JSONL. Hermes schreibt „Was wusste ich damals noch nicht"-Diff.

---

## 🔮 Predictive Research

**Konzept:** Nicht auf News reagieren, sondern antizipieren.

Analyse von:
- Suchvolumen-Trends (Google Trends via SearXNG)
- Publikations-Frequenz (ArXiv, RSS-Feeds)
- Cross-Reference-Wachstum (Wer zitiert wen plötzlich?)

**Output:** Wöchentlicher Report: „Diese 3 Themen werden nächste Woche relevant".

---

## 🤖 LLM Summary

**Konzept:** Top-N Suchergebnisse von Hermes zusammenfassen lassen.

**Trigger:** Per Telegram `/summary <query>` oder automatisch nach Self-Improving Loop.

**Output:** 3-Satz-Abstract + Relevanz-Bewertung + „Das fehlt noch"-Hinweis.

**Spart Perplexity-Tokens** für Routinekram.

---

## 🚀 Perplexity Deep-Dive

**Konzept:** Wenn eine Recherche tiefer gehen muss: Query + Top-3-Quell-URLs automatisch an Perplexity schicken.

**Trigger:** Hermes erkennt „das ist komplex, Perplexity lohnt sich" — oder manuell per `/deep <query>`.

**Output:** Perplexity-Antwort mit vor-geladenem Kontext → bessere Qualität, weniger Halluzination.

---

## 🗂️ Implementierungs-Plan

| Prio | Feature | Aufwand | Abhängigkeiten |
|------|---------|---------|----------------|
| 1 | LLM Summary | 1 Skill | Hermes + SearXNG |
| 2 | Self-Improving Loop | 1 Skill + 1 Cron | LLM Summary |
| 3 | Knowledge Graph | 2 Skills + JSON-DB | Self-Improving Loop |
| 4 | Adversarial Search | 1 Skill | Perplexity API |
| 5 | Emergence Detection | 1 Cron (daily) | Embedding API |
| 6 | Research Replay | 1 Skill + JSONL | Session-Tracking |
| 7 | Predictive Research | 1 Cron (weekly) | Emergence Detection |
| 8 | Perplexity Deep-Dive | 1 Skill | Perplexity API |
