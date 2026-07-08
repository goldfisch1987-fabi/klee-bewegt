# design-alt Polish — Implementierungsplan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Vier chirurgische CSS-Fixes in `index.html` auf dem `design-alt` Branch umsetzen — Logo-Rahmen integrieren, SOON-Typografie auf Weiß umstellen, Overlay-Kontrast erhöhen, Abstände straffen.

**Architecture:** Einzige Datei ist `index.html`. Alle Änderungen erfolgen im `<style>`-Block. Kein Build-Schritt, kein Framework, kein JavaScript-Eingriff. Visuelle Verifikation im Browser statt automatisierter Tests.

**Tech Stack:** Vanilla HTML5 · CSS custom properties · OKLCH-Farben · Google Fonts (Barlow, Barlow Condensed)

## Global Constraints

- Branch: `design-alt` — niemals auf `main` arbeiten
- Nur `index.html` modifizieren — keine neuen Dateien erstellen
- Farben ausschließlich in OKLCH
- Keine HTML-Struktur ändern — keine Elemente hinzufügen oder entfernen
- Impressum-Modal (Inhalt und JS) unberührt lassen
- WCAG AA: bestehende Kontrastverhältnisse nicht verschlechtern
- Lokaler Testserver: `http://localhost:8787` (via `npx serve` im Projektverzeichnis)

---

### Task 1: Logo-Rahmen integrieren

**Files:**
- Modify: `index.html` — `.logo`-Regel im `<style>`-Block

**Interfaces:**
- Consumes: `.logo`-Klasse, aktuell `width: min(280px, 60vw); height: auto; object-fit: contain;`
- Produces: `.logo` mit `border-radius` und `box-shadow` — alle nachfolgenden Tasks können das Logo so lassen

- [ ] **Schritt 1: Aktuelle `.logo`-Regel lokalisieren**

In `index.html` die folgende Regel finden (suche nach `.logo {`):

```css
.logo {
  width: min(280px, 60vw);
  height: auto;
  object-fit: contain;
}
```

- [ ] **Schritt 2: Regel ersetzen**

Ersetze die gesamte `.logo`-Regel durch:

```css
.logo {
  width: min(280px, 60vw);
  height: auto;
  object-fit: contain;
  border-radius: 10px;
  box-shadow: 0 8px 32px oklch(0 0 0 / 0.55);
}
```

- [ ] **Schritt 3: Visuell prüfen**

Browser öffnen: `http://localhost:8787`

Erwartung:
- Das Logo hat abgerundete Ecken (sichtbar an den vier Ecken des schwarzen Kastens)
- Ein weicher Schlagschatten ist sichtbar (dunkler Schein unter/um den Kasten)
- Der Kasten wirkt wie eine absichtliche Karte, nicht wie ein Fehler

- [ ] **Schritt 4: Committen**

```bash
git add index.html
git commit -m "style(alt): Logo-Rahmen mit border-radius und box-shadow integriert"
```

---

### Task 2: SOON-Typografie — Outline auf Weiß umstellen

**Files:**
- Modify: `index.html` — `h1 .word-soon`-Regel im `<style>`-Block

**Interfaces:**
- Consumes: `h1 .word-soon` aktuell mit `color: transparent; -webkit-text-stroke: 2px var(--green);`
- Produces: `h1 .word-soon` mit `color: var(--ink)` — beide H1-Wörter sind weiß gefüllt

- [ ] **Schritt 1: Aktuelle `.word-soon`-Regel lokalisieren**

In `index.html` die folgende Regel finden (suche nach `.word-soon`):

```css
h1 .word-soon {
  display: block;
  color: transparent;
  -webkit-text-stroke: 2px var(--green);
}
```

- [ ] **Schritt 2: Regel ersetzen**

Ersetze die gesamte `h1 .word-soon`-Regel durch:

```css
h1 .word-soon {
  display: block;
  color: var(--ink);
}
```

- [ ] **Schritt 3: Visuell prüfen**

Browser neu laden: `http://localhost:8787`

Erwartung:
- "COMING" und "SOON" sind beide vollständig weiß ausgefüllt
- Kein grüner Outline-Stroke sichtbar
- Grüne Akzente nur noch: Eyebrow-Label "NEUE WEBSITE" und der horizontale Divider
- Die Headline wirkt ruhiger und lesbarer

- [ ] **Schritt 4: Committen**

```bash
git add index.html
git commit -m "style(alt): SOON-Outline entfernt, beide H1-Woerter weiss"
```

---

### Task 3: Overlay-Gradient für besseren Textkontrast verstärken

**Files:**
- Modify: `index.html` — `body`-Regel, `background-image`-Property im `<style>`-Block

**Interfaces:**
- Consumes: `body` background-image mit Gradient + Foto-URL
- Produces: Gradient mit 0.78 Opacity bei 55% (statt 0.70) — gleiche Struktur, höherer Mittelwert

- [ ] **Schritt 1: Gradient-Wert lokalisieren**

In `index.html` im `body`-Block den `background-image`-Gradient finden:

```css
linear-gradient(
  to bottom,
  oklch(0.06 0 0 / 0.45) 0%,
  oklch(0.06 0 0 / 0.70) 55%,
  oklch(0.06 0 0 / 0.88) 100%
),
```

- [ ] **Schritt 2: Mittelwert erhöhen**

Den Wert `0.70` bei `55%` auf `0.78` ändern:

```css
linear-gradient(
  to bottom,
  oklch(0.06 0 0 / 0.45) 0%,
  oklch(0.06 0 0 / 0.78) 55%,
  oklch(0.06 0 0 / 0.88) 100%
),
```

Nur diese eine Zahl ändert sich. Die Foto-URL und `background-attachment: fixed` bleiben unberührt.

- [ ] **Schritt 3: Visuell prüfen**

Browser neu laden: `http://localhost:8787`

Erwartung:
- Der Textbereich (Eyebrow → H1 → Divider → Subtitle → Kontakt) hat mehr Kontrast gegenüber dem Foto
- Das Foto ist noch erkennbar (kein Vollblack-Overlay)
- Weiße Texte sind klar lesbar auch gegen hellere Bildpartien

- [ ] **Schritt 4: Committen**

```bash
git add index.html
git commit -m "style(alt): Overlay-Gradient bei 55% auf 0.78 verstaerkt"
```

---

### Task 4: Vertikale Abstände straffen

**Files:**
- Modify: `index.html` — fünf CSS-Regeln im `<style>`-Block

**Interfaces:**
- Consumes: `.eyebrow`, `h1`, `.divider`, `.subtitle`, `.site-header` — aktuell mit größeren clamp()-Werten
- Produces: dieselben Regeln mit engeren clamp()-Werten; Gesamthöhe des Inhalts passt in 1280×800 ohne Scrollen

- [ ] **Schritt 1: `.site-header` padding straffen**

Aktuelle Regel finden (`padding:` im `.site-header`):

```css
.site-header {
  padding: clamp(1.5rem, 4vw, 2.5rem) clamp(1.5rem, 5vw, 3rem);
  ...
}
```

`padding`-Wert ersetzen (nur die erste clamp-Angabe — oben/unten):

```css
.site-header {
  padding: clamp(1rem, 3vw, 1.75rem) clamp(1.5rem, 5vw, 3rem);
  ...
}
```

- [ ] **Schritt 2: `.eyebrow` margin-bottom straffen**

Aktuelle Regel:

```css
margin-bottom: clamp(0.75rem, 2vw, 1rem);
```

Ersetzen durch:

```css
margin-bottom: clamp(0.5rem, 1.5vw, 0.75rem);
```

- [ ] **Schritt 3: `h1` margin-bottom straffen**

Aktuelle Regel:

```css
margin-bottom: clamp(1rem, 2.5vw, 1.75rem);
```

Ersetzen durch:

```css
margin-bottom: clamp(0.75rem, 2vw, 1.25rem);
```

- [ ] **Schritt 4: `.divider` margin-bottom straffen**

Aktuelle Regel:

```css
margin: 0 auto clamp(1rem, 2vw, 1.5rem);
```

Ersetzen durch:

```css
margin: 0 auto clamp(0.75rem, 1.5vw, 1rem);
```

- [ ] **Schritt 5: `.subtitle` margin-bottom straffen**

Aktuelle Regel:

```css
margin: 0 auto clamp(1.5rem, 3vw, 2rem);
```

Ersetzen durch:

```css
margin: 0 auto clamp(1rem, 2vw, 1.5rem);
```

- [ ] **Schritt 6: Visuell prüfen**

Browser-Fenster auf 1280×800 px setzen (DevTools → Responsive Mode) und `http://localhost:8787` neu laden.

Erwartung:
- Logo, Eyebrow, H1, Divider, Subtitle und Kontakt-E-Mail sind alle ohne Scrollen vollständig sichtbar
- Kein Element wird abgeschnitten
- Die Abstände wirken kompakt aber nicht gedrängt

- [ ] **Schritt 7: Auch auf 1920×1080 prüfen**

Browser-Fenster auf 1920×1080 setzen.

Erwartung:
- Alle Inhalte sichtbar, kein übermäßiger Leerraum
- Symmetrischer vertikaler Weißraum ober- und unterhalb des Inhaltsblocks

- [ ] **Schritt 8: Committen**

```bash
git add index.html
git commit -m "style(alt): Vertikale Abstaende gestrafft, alles in 1280x800 sichtbar"
```

---

### Task 5: Push und Vercel-Preview prüfen

**Files:**
- Keine Dateiänderung — nur git push

**Interfaces:**
- Consumes: alle vier Commits aus Tasks 1–4
- Produces: aktualisierte Preview-URL `https://klee-bewegt-git-design-alt-fabian1987.vercel.app`

- [ ] **Schritt 1: Auf GitHub pushen**

```bash
git push origin design-alt
```

Erwartung: Vercel triggert automatisch ein neues Deployment.

- [ ] **Schritt 2: Deployment abwarten**

Im Vercel Dashboard unter `fabian1987/klee-bewegt → Deployments` warten bis der neueste `design-alt`-Eintrag den Status "Ready" zeigt (typisch: 10–30 Sekunden).

- [ ] **Schritt 3: Preview-URL prüfen**

`https://klee-bewegt-git-design-alt-fabian1987.vercel.app` im Browser öffnen.

Sichtprüfung:
- [ ] Logo mit abgerundeten Ecken und Schatten
- [ ] "COMING" und "SOON" beide weiß, kein Outline
- [ ] Foto sichtbar aber Text klar lesbar
- [ ] Gesamtinhalt ohne Scrollen sichtbar (auf Desktop)
- [ ] Footer mit Impressum-Link und Foto-Credit
- [ ] Impressum-Modal öffnet sich per Klick
