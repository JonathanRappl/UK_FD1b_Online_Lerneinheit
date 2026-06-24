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
- **Phase 2 „Von der Zahl zum Buchstaben" umgesetzt** (`phases/phase2.qmd`): knüpft an 65→»A«
  an, erklärt ASCII als Vereinbarung (Tabelle, Buchstaben lückenlos). Aufgabe 3 (Predict: `C`=67,
  Zahlen-Check + Lösung), Merke (`A`=65/`a`=97, Vereinbarung), Ausprobieren mit neuem
  **ASCII-Encoder-Widget** (`encode-widget`: Texteingabe → Live-Byte-Grid via `_renderEncode`,
  reuse `_byteCell`; eigenen Namen codieren = Kreativität), Aufgabe 4 (Decode 72·97·108·108·111 →
  „Hallo", neuer **`checkText`**-Prüfer via `data-check="text"`, Hint für „halo"), Hilfe
  (Buchstaben-Zahlen-Tabelle zum Nachschlagen), Challenge (`a`−`A`=32 = genau ein Bit, Brücke zu
  den Stellenwerten). Schluss leitet zu Bildern/Pixeln (Phase 3) über. `_quarto.yml` (render +
  sidebar) erweitert. Voll-Render fehlerfrei.
- **Phase 3 „Vom Pixel zum Bild" umgesetzt** (`phases/phase3.qmd`): Bild als Pixel-Raster.
  Schwarz-Weiß = 1 Bit/Pixel; dann RGB = drei Bytes/Pixel (0–255, Brücke zum Byte). Drei NEUE
  Widgets in `widgets.html`: (1) `pixel-widget` (`_renderPixel`/`setPixel`, Stepper „als Zahlen /
  als Bild" wie Reveal-Stepper) für Aufgabe 5 (Predict 5×5-Raster → Raute), (2) `pixel-draw`
  (`_renderDraw`/`togglePixel`, editierbares 8×8-Raster → Live-Bitfolge) als Kreativ-Ausprobieren
  (eigenes Pixelbild), (3) `rgb-widget` (`_renderRgb`, drei Slider R/G/B → Farbfeld + drei
  Bytes/Binär) als Farb-Ausprobieren. Aufgabe 6 (MC: reines Rot = 255·0·0), Hilfe (Reihenfolge
  R·G·B, 0/255), Challenge (MC: Rot+Grün-Licht = Gelb, additive Lichtmischung ≠ Tuschkasten,
  255·255·255 = Weiß). Aufgaben jetzt bei 6. CSS für `.pixel-*` und `.rgb-*` ergänzt. Schluss
  leitet zum Python-Climax (Phase 4, PRIMM) über. `_quarto.yml` erweitert. Voll-Render fehlerfrei.

## 2026-06-23

- **Phase 4 „Vom Code zum Bild" umgesetzt** (`phases/phase4.qmd`) — der PRIMM-Climax. Statt jedes
  Bit von Hand zu setzen, erzeugen die SuS ihr Pixelbild mit Python. **Ausführung extern über
  WebTigerPython** (`https://webtigerpython.ethz.ch/`, Copy-Paste) statt eingebettetem Pyodide —
  konsistent mit dem bestehenden `Quarto_Scripts`-Ansatz (vgl. `kryptologie/xor-stromchiffre.qmd`),
  leichtgewichtig, kein 6-MB-CDN-Download. Eine grüne „Ausprobieren"-Box erklärt den Open-Paste-Run-
  Workflow einmalig.
- **PRIMM-Strecke** als Callout-Folge: *Predict* = Aufgabe 7 (Programm nur lesen, Ausgabe auf Papier
  vorhersagen → malt ein »A«, Rückbezug aufs A=65-Leitmotiv der ganzen Einheit + aufs 0/1-Raster aus
  Phase 3; Lösung eingeklappt). *Investigate* = Aufgabe 8 mit **zwei MC-Widgets** (`mc-newline`,
  `mc-end`, via bestehendem `mcCheck`) zum Code-Verständnis (`print()`-Zeilenumbruch, `end=""`).
  *Modify* = Aufgabe 9 (Liste so ändern, dass ein »T« erscheint; Lösung eingeklappt). *Make* =
  Aufgabe 10 (eigenes Pixelbild entwerfen — Skizze/Papier oder das `pixel-draw`-Raster aus Phase 3
  nutzen, dann als 0/1-Zeilen übertragen; Partnerarbeit/Erraten = Kreativität). Hilfe-Callout erklärt
  die zwei Schleifen; Merke-Callout (Programm = Regel statt Handarbeit, Bild = Raster aus Zahlen).
- **Challenge: Graustufen** — Programm mit Ziffern 0–9 + Paletten-String (`stufen[int(ziffer)]`),
  malt eine schattierte Raute. Brücke: mehr Stufen = mehr Bits/Pixel → 256 Stufen = ein Byte →
  Farbfoto = drei Bytes. Schließt den Kreis zum Byte-Begriff. **Kein neues Widget/CSS nötig** (nur
  Wiederverwendung von `mcCheck` + Standard-Codeblöcke; `code-fold` betrifft nur `{python}`-Zellen,
  reine ```python-Blöcke bleiben sichtbar mit Kopier-Button).
- **Abschluss-Absatz** rekapituliert den roten Faden (Binär-Nachricht → Bit→Zahl → Zahl→Buchstabe →
  Pixel→Bild → Code malt Bild) und schließt mit „Alles ist Zahl". `_quarto.yml` (render + sidebar)
  um `phases/phase4.qmd` erweitert (vor `transparenz.qmd`). Voll-Render (7/7) fehlerfrei,
  phase3→phase4-Navigation greift.
- **Phase 5 „Geschafft!" umgesetzt** (`phases/phase5.qmd`) — die Abschluss-Station. Zweck:
  ARCS-„Satisfaction", Konsolidierung/Wiederholung (Übungsprinzip) und Ausblick. (1) **Bogen zum
  Anfang**: grüne „Ausprobieren"-Box mit dem `reveal-widget` (`data-secret="Alles ist Zahl"`,
  Stepper „die Bits / die Zahlen / die Zeichen") — dieselbe Geheimnachricht wie in Phase 0, jetzt
  versteht man jeden Schritt. (2) **Merke** (roter Faden kompakt: Bits→Byte→256→ASCII/Pixel/RGB) +
  „Das kannst du jetzt"-Liste (Bloom: anwenden). (3) **Aufgabe 11 „Selbsttest"** mit vier
  gemischten Quer-Checks (Widgets wiederverwendet, keine neuen): a) `00101010`=42 (Zahlen-Check,
  Hint für Bits-Antwort), b) »B«=66 (Zahlen-Check, Hint für 65), c) MC 1 Bit/SW-Pixel, d) MC
  3 Bytes/RGB-Pixel. (4) **Challenge „Wie es weitergeht"** = Ausblick/Differenzierung für
  Neugierige: Lauflängen-Codierung interaktiv (`0000000011111111` → wie viele Zahlen? Antwort 4 =
  Wert+Anzahl × 2 Blöcke, Hint für die naheliegende 2), plus Kurz-Teaser UTF-8 (Rückbezug auf
  256-Grenze / 中❤😀 aus Phase 0) und Fehlererkennung/Prüf-Bit. (5) **„Zum Nachdenken"** (grün) =
  kurze Reflexion. Schluss verweist auf die Transparenz-Seite. Aufgaben jetzt bei 11. `_quarto.yml`
  (render + sidebar) um `phases/phase5.qmd` vor `transparenz.qmd` erweitert. Voll-Render (8/8)
  fehlerfrei, phase4→phase5→transparenz-Navigation greift. Damit ist der Kernpfad der Einheit
  komplett (Phase 0–5).
