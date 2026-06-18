# Arbeitsjournal

Sehr reduziertes Protokoll der Arbeitsschritte, groben Änderungen und Überlegungen, damit der
Arbeitsprozess nachvollziehbar bleibt. Neueste Einträge oben.

## 2026-06-18

- **Thema festgelegt:** Datenrepräsentation / „Bits & Bytes". Idee: bereits unterrichtetes
  (analoges) Material in eine interaktive Quarto-Online-Einheit übersetzen. Doppeltes Projektziel:
  (1) Grenzen der Interaktivität von Quarto + GitHub Pages ausloten, (2) bestehendes Material
  rekontextualisieren.
- **Grobe Outline** entworfen: roter Faden „Alles ist Zahl" — Bit→Zahl→ASCII→Pixel/RGB→
  eigenes Pixel-Werk per Python (PRIMM-Climax)→Abschluss; Zusatzposten RLE/Huffman/UTF-8/Parity.
  Kernpfad ~60 min, Zusatzposten als Challenge für Schnelle.
- **Stilkonvention** festgelegt: meiste Inhalte in Callouts. Aufgabe=note (blau),
  Hilfe/Challenge=caution (orange), Ausprobieren=tip (grün), Merke=important (rot),
  Lösung=eingeklappter note. Header sparsam (1× `##` pro Phase). Keine Callout-Icons.
- **Grundgerüst gebaut:** `_quarto.yml` (Website, Output `_site`), `index.qmd` (Intro + Legende +
  `include` der Phasen), `styles.css`, `.github/workflows/publish.yml` (Action → gh-pages),
  `.gitignore`. Interaktivität via ObservableJS (clientseitig, läuft auf GitHub Pages).
- **Phase 0 „Eine geheime Nachricht"** umgesetzt: Hook mit Binär-Nachricht „Alles ist Zahl",
  OJS-Stepper zum schrittweisen Aufdecken, Aufgabe 1 (Vermuten/Paararbeit), Challenge mit
  Eingabefeld (Zahl prüfen) + Ja/Nein-Frage, je mit hilfreichem Feedback.
- **Transparenz** ergänzt: Footer-Hinweis + Abschnitt „Transparenz & Quellen" (bestehendes
  Material, KI-Unterstützung durch Claude, Werkzeug/Hosting).
- **Feedback eingearbeitet:** Begriff „Differenzierung" durch „Hilfe"/„Challenge" ersetzt;
  Callout-Icons global entfernt; Eingabefelder mit Auto-Feedback bei Aufgaben mit klarer Lösung;
  Stift-und-Papier-Hinweis am Anfang; Stepper-Labels weniger verräterisch gemacht (Aufgabenstellung
  soll nicht zu viel vorwegnehmen).
