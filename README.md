# Minecraft Gilde API

JSON-API für **Minecraft-Gilde.de**. Liefert Leaderboards, Spieler-Statistiken und eine gecachte Proxy-Antwort für das Mojang Sessionserver-Profil (Skins/Capes).

- **Runtime:** PHP (ohne Framework)
- **Datenquelle:** MariaDB/MySQL (Views/Tabellen werden typischerweise vom Importer befüllt)
- **HTTP:** Nur **GET** und **OPTIONS** (Preflight)

---

## Endpunkte

> Basis-URL (typisch): `https://minecraft-gilde.de/api/`
>
> Dank `.htaccess` wird alles auf `index.php` geroutet. Alternativ kann (ohne Rewrite) auch `index.php?route=...` genutzt werden.

### `GET /metrics`
Metadaten zu aktivierten Metriken (Labels, Kategorien, Einheiten, Sortierung, optional Divisor/Decimales).

**Antwort**
- `metrics`: Map `{ metricId: { label, category, unit, sort_order, divisor?, decimals? } }`
- `__generated`: ISO-8601 Timestamp (UTC) der aktiven Import-Generation

---

### `GET /summary?metrics=...`
Leichtgewichtige Übersicht: Spieleranzahl + Summen für ausgewählte Metriken.

**Query**
- `metrics` (required): Komma-separierte Liste von Metric-IDs (max. 12)

**Antwort**
- `player_count`: Anzahl Spieler
- `totals`: `{ metricId: total }`
- `__generated`

Beispiel:
```bash
curl "https://minecraft-gilde.de/api/summary?metrics=hours,distance,mob_kills"
