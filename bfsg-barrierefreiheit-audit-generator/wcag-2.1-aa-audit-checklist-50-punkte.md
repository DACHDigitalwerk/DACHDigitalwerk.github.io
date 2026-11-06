# BFSG 2026 / WCAG 2.1 AA / EN 301 549 ? 50-Punkte Entwickler- & Agentur-Checkliste
**Herausgeber:** DACH Digitalwerk (https://dachdigitalwerk.github.io/bfsg-barrierefreiheit-audit-generator/)

## 1. Wahrnehmbarkeit (Perceivable)
- [ ] Alle informativen Bilder (`<img>`), Icons und SVGs besitzen pr?zise `alt`-Attribute; rein dekorative Grafiken tragen `alt=""` oder `aria-hidden="true"`.
- [ ] Farbkontrast f?r normalen Text mindestens **4.5:1** (WCAG AA) bzw. **7.0:1** (WCAG AAA); f?r gro?en Text (ab 18pt / 14pt fett) mindestens **3.0:1**.
- [ ] UI-Komponenten, Formularr?nder und Status-Icons erreichen mindestens **3.0:1** Kontrast zum Hintergrund.
- [ ] Schriftgr??e l?sst sich im Browser bis auf **200 %** skalieren, ohne dass Text abgeschnitten wird oder horizontales Scrollen auf 320 CSS-Pixeln entsteht (Reflow 1.4.10).
- [ ] Informationen (z. B. Pflichtfelder, Fehlermeldungen, Lagerbestand) werden niemals ausschlie?lich ?ber Farbe vermittelt.

## 2. Bedienbarkeit (Operable)
- [ ] Vollst?ndige Tastaturbedienbarkeit (`Tab`, `Shift+Tab`, `Enter`, `Space`, `Escape`, Pfeiltasten) ohne Tastaturfallen (Keyboard Traps).
- [ ] Sichtbarer Fokus-Indikator (`:focus-visible`) mit mindestens 2px Kontrastrahmen auf allen interaktiven Elementen.
- [ ] Funktionierender **Skip-to-Content-Link** ("Direkt zum Hauptinhalt springen") als erstes fokussierbares Element im DOM.
- [ ] Modale Dialoge und Cookie-Banner fangen den Tastaturfokus im ge?ffneten Zustand ein und geben ihn beim Schlie?en via `Escape` an das ausl?sende Element zur?ck.
- [ ] Touch-Ziele auf Mobilger?ten haben eine Mindestgr??e von **44 ? 44 CSS-Pixeln** (bzw. 24 ? 24 px Mindestabstand nach WCAG 2.2).

## 3. Verst?ndlichkeit (Understandable)
- [ ] Korrektes `lang`-Attribut im `<html>`-Tag (`lang="de"`, `lang="de-AT"`, `lang="de-CH"`).
- [ ] Jedes `<input>`, `<select>` und `<textarea>` ist programmatisch ?ber `<label for="...">` oder `aria-labelledby` verkn?pft (Placeholder allein gen?gt nicht!).
- [ ] Fehlermeldungen in Checkout- und Kontaktformularen werden via `aria-invalid="true"` und `aria-describedby` direkt am Feld angek?ndigt.

## 4. Robustheit & ? 14 BFSG Pflichtdokumentation
- [ ] Semantische HTML5-Landmarken (`<header>`, `<nav>`, `<main id="main-content">`, `<footer>`) und l?ckenlose ?berschriftenhierarchie (`h1` -> `h2` -> `h3`).
- [ ] Ver?ffentlichte **Barrierefreiheitserkl?rung nach ? 14 BFSG & Anlage 3** inklusive Feedback-Mechanismus und zust?ndiger Markt?berwachungsbeh?rde (MLBF).