# Arbeitsjournal

Sehr reduziertes Protokoll der Arbeitsschritte, groben Änderungen und Überlegungen, damit der
Arbeitsprozess nachvollziehbar bleibt.

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
- **Interaktivität von ObservableJS auf Vanilla-JS umgestellt.** Grund: OJS-Eingabefeld
  reagierte beim Nutzer nicht (OJS lädt `Inputs` per ESM aus dem Netz — schlägt u. a. beim Öffnen
  über `file://` fehl). Vanilla-JS hat keine solche Abhängigkeit und läuft überall. Feedback-Stil
  am bestehenden SQL-Beispiel orientiert (Quarto_Scripts, Branch `sql_test`, `sql/index.qmd`):
  grüne `show-correct` / gelbe `show-wrong` Boxen mit farbigem Rand und ausführlichem Text + Tipp.
- **Wiederverwendbare Widgets** angelegt in `widgets.html` (via `include-after-body`):
  Aufdeck-Stepper (`setReveal`), Zahlen-Check (`checkNumber`, mit gezielten Hinweisen je
  Fehlantwort über `.msg-hint[data-value]`), Multiple-Choice (`mcCheck`, wie SQL). Styles in
  `styles.css`. Phase 0 entsprechend umgebaut.
- **Phase 1 „Vom Bit zur Zahl" umgesetzt:** Stellenwert-System erklärt (Analogie 365 im
  Zehnersystem → Binär: jede Stelle ×2 statt ×10). Neues interaktives Widget `binary-widget`
  (`toggleBit`/`_renderBinary`): 8 anklickbare Bits mit Stellenwert-Köpfen (128…1), Summe live.
  Aufgabe 2 (Predict auf Papier: `01000001` = 65, Zahlen-Check + eingeklappte Lösung), Merke
  (Bitfolge = Zahl im Zweiersystem), Hilfe (4-Bit-Beispiel `1011`=11), Challenge (größte Zahl
  255, Bezug zu den 256 Möglichkeiten aus Phase 0). Schluss leitet zu ASCII über (65→»A«).
  In `index.qmd` eingebunden, rendert fehlerfrei.
- **Von Scroll-Seite auf seitenweise Navigation umgestellt.** Statt aller Phasen per
  `{{< include >}}` auf einer langen Seite ist nun jede Station eine **eigene Seite**
  (`index.qmd` = Start/Legende, `phases/phase0.qmd`, `phases/phase1.qmd`, `transparenz.qmd`).
  Reihenfolge über `website.sidebar.contents` in `_quarto.yml`; `page-navigation: true` erzeugt
  am Seitenende automatisch verlinkte Weiter/Zurück-Pfeile (zeigen den jeweiligen Kapiteltitel).
  Phasen-Dateien ohne Unterstrich, `##`-Überschrift → Frontmatter-`title`. Widgets/Styles
  (global via `include-after-body`) und Deployment unverändert. Rendert fehlerfrei.
