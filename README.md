# FISI-Prüfungsfragen – offener Fragen-Datensatz

Rund **4.200 Übungsfragen, 15 Musterprüfungen, 200 Glossarbegriffe und 150 Lernkarten** zur
Vorbereitung auf die IHK-Abschlussprüfungen der IT-Berufe (Schwerpunkt **Fachinformatiker:in
Systemintegration**, dazu Anwendungsentwicklung und WiSo) – als schlichte JSON-Dateien.

Gedacht für eigene Quiz-Apps, Anki-Decks, Discord-Bots, Lernplattformen oder Auswertungen.
Die Daten stammen aus der Lernplattform **FISI-HQ** (eigenes Repository).

## Dateien (`data/`)

| Datei | Inhalt | Anzahl |
|---|---|---|
| `uebungsfragen.json` | Kernfragen aller drei Ausbildungsjahre | 52 |
| `ap1-fragen.json` | Fragen im Stil AP Teil 1 | 60 |
| `ap2-fisi-fragen.json` | Fragen im Stil AP Teil 2 FISI | 50 |
| `ap2-fiae-fragen.json` | Fragen im Stil AP Teil 2 FIAE | 30 |
| `lernfeld-fragen.json` | Single-Choice-Fragen je Lernfeld/Lektion (LF1–LF12a, WiSo, Zertifikate …) | 3.683 |
| `musterpruefungen.json` | 15 komplette Prüfungsbögen mit Situation, Teilaufgaben, Punkten, Musterlösung | 15 |
| `glossar.json` | Fachbegriffe mit einfacher und vertiefter Erklärung | 201 |
| `lernkarten.json` | Karteikarten (Vorder-/Rückseite) | 150 |
| `index.json` | Version und Anzahl je Datei | – |

## Fragenformat

Alle Fragen haben `id`, `q` (Fragetext, kann einfaches HTML enthalten) und `explain` (Begründung).
Je nach `type`:

| `type` | Felder | Richtig ist … |
|---|---|---|
| `single` | `options[]`, `correct` (Index) | `options[correct]` |
| `mc` | `options[]`, `correct[]` (Indizes) | alle `options[i]` mit `i` in `correct` |
| `gap` | `accept[]` | jede Eingabe aus `accept` (Groß-/Kleinschreibung und Leerzeichen ignorieren) |
| `order` | `items[]` | genau die gespeicherte Reihenfolge (oben → unten) – zum Abfragen mischen |
| `match` | `pairs[]` mit `l`/`r` | jedes `l` gehört zu seinem `r` – rechte Seite zum Abfragen mischen |

Optionale Felder: `topic`, `year` (Ausbildungsjahr), `rel` (`high`/`med` = Prüfungsrelevanz),
`hint`, `mistake` (typischer Denkfehler), `simpler` (vereinfachte Teilaufgabe, HTML).

`lernfeld-fragen.json` zusätzlich: `lernfeld`, `lernfeld_nr`, `thema`, `lektion`.

**Tipp:** Antwortoptionen beim Anzeigen immer mischen – die richtige Antwort ist in den Rohdaten
überdurchschnittlich oft die längste bzw. steht oft an Position B.

### Musterprüfungen

```json
{ "wiso": { "kurz": "WiSo", "titel": "…", "untertitel": "60 Minuten · 100 Punkte …", "zeit": 60,
  "situation": "…",
  "aufgaben": [ { "t": "Aufgabe 1: … (20 Punkte)", "teile": [
    { "k": "1.1", "frage": "…", "punkte": 4, "optionen": ["…"], "richtig": 1, "loesung": "…" },
    { "k": "2.3", "frage": "…", "punkte": 6, "zeilen": 4, "loesung": "Bewertungsraster …" } ] } ] } }
```

Teile mit `optionen` sind gebunden (Index in `richtig`), Teile mit `tabelle` haben in `tabelle.check`
die erwarteten Werte (Alternativen mit `|` getrennt), alle anderen sind offene Aufgaben mit Musterlösung.

## Beispiel (JavaScript)

```js
const fragen = await (await fetch("data/lernfeld-fragen.json")).json();
const lf3 = fragen.filter(f => f.lernfeld === "lf3");
const f = lf3[Math.floor(Math.random() * lf3.length)];
console.log(f.q, f.options, "→", f.options[f.correct]);
```

## Hinweise

- **Keine Original-Prüfungsaufgaben** und kein offizielles Material der IHK oder ZPA – alle Aufgaben sind
  eigenständig formuliert.
- Mit KI-Unterstützung erstellt und redaktionell geprüft; Fehler sind möglich. Korrekturen gern per Issue/PR.
- Rechtsstand der WiSo-Fragen: 2026. Gesetzesänderungen (z. B. Beitragsgrenzen, Mindestvergütung) vor
  Verwendung prüfen.

## Lizenz

MIT – siehe [`LICENSE`](LICENSE). Namensnennung „FISI-Prüfungsfragen“ freut uns, ist aber über den
Lizenzhinweis hinaus nicht nötig.
