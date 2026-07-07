# Coming Soon Redesign — Implementierungsplan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** `index.html` zur "Monumentalen Stille"-Version umbauen — Logo dominiert, alle Ablenkungen weg, Hintergrund und Licht neu, Typografie gestrafft.

**Architecture:** Einzige Datei ist `index.html`. Alle Änderungen erfolgen inline (CSS im `<style>`-Block, HTML im `<body>`). Kein Build-Schritt, kein Framework.

**Tech Stack:** Vanilla HTML5 · CSS custom properties · Google Fonts (Barlow, Barlow Condensed) · kein JavaScript außer dem bestehenden Impressum-Modal

## Global Constraints

- Sprache: Deutsch (`lang="de"`)
- Schriften: Barlow + Barlow Condensed von Google Fonts — nicht wechseln
- Farben ausschließlich in OKLCH
- Responsivität mit `clamp()` — kein festes `px`-Breakpoint-System
- Impressum-Modal: Inhalt und JS unverändert
- WCAG AA: ausreichend Kontrast auf schwarzem Hintergrund
- Keine externen Assets außer `logo.jpeg` (bereits vorhanden)
- Animation nur unter `prefers-reduced-motion: no-preference`

---

### Task 1: CSS-Variablen und Hintergrundfarbe aktualisieren

**Files:**
- Modify: `index.html` — `:root`-Block im `<style>`-Tag

**Interfaces:**
- Produces: aktualisierte CSS-Custom-Properties, die alle nachfolgenden Tasks verwenden

- [ ] **Schritt 1: `:root`-Block ersetzen**

Ersetze den gesamten `:root`-Block (Zeilen 14–24) durch:

```css
:root {
  --green:        oklch(0.58 0.18 145);
  --green-light:  oklch(0.68 0.18 145);
  --green-glow:   oklch(0.58 0.18 145 / 0.2);
  --ink:          oklch(0.93 0 0);
  --ink-muted:    oklch(0.55 0 0);
  --ink-subtle:   oklch(0.38 0 0);
  --bg:           oklch(0.06 0 0);
  --bg-elevated:  oklch(0.12 0.005 145);
  --border:       oklch(0.20 0.005 145);
}
```

- [ ] **Schritt 2: Visuell prüfen**

`index.html` im Browser öffnen (Datei direkt, kein Server nötig).
Erwartung: Hintergrund ist jetzt fast reines Schwarz (deutlich dunkler als vorher), Grün wirkt kräftiger.

- [ ] **Schritt 3: Committen**

```bash
git add index.html
git commit -m "style: CSS-Variablen auf Redesign-Werte aktualisiert"
```

---

### Task 2: Hintergrund-Lichtstrahl ersetzen

**Files:**
- Modify: `index.html` — `body::before`-Regel im `<style>`-Tag

**Interfaces:**
- Consumes: `--green-glow` aus Task 1
- Produces: horizontaler Lichtstrahl hinter dem Logo-Bereich

- [ ] **Schritt 1: `body::before` ersetzen**

Ersetze die `body::before`-Regel (aktuell Zeilen 41–51) durch:

```css
body::before {
  content: '';
  position: fixed;
  top: 36%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 100vw;
  height: 160px;
  background: radial-gradient(
    ellipse 70% 80px at 50% 50%,
    var(--green-glow) 0%,
    transparent 100%
  );
  pointer-events: none;
}
```

- [ ] **Schritt 2: Visuell prüfen**

Browser neu laden.
Erwartung: Statt des diffusen Glows ist jetzt ein schmaler, horizontaler Lichtstrahl sichtbar — er liegt hinter dem Logo-Bereich wie ein Scheinwerfer von der Seite.

- [ ] **Schritt 3: Committen**

```bash
git add index.html
git commit -m "style: diffusen Glow durch horizontalen Lichtstrahl ersetzt"
```

---

### Task 3: Logo-Größe erhöhen, drop-shadow entfernen

**Files:**
- Modify: `index.html` — `.logo`-Regel im `<style>`-Tag

**Interfaces:**
- Produces: zentrales, unverfälschtes Logo-Element ohne Glow-Effekt

- [ ] **Schritt 1: `.logo`-Regel ersetzen**

Ersetze die `.logo`-Regel durch:

```css
.logo {
  width: min(600px, 92vw);
  margin-bottom: clamp(2rem, 5vw, 3rem);
}
```

- [ ] **Schritt 2: Visuell prüfen**

Browser neu laden.
Erwartung: Logo ist größer und steht klar auf dem schwarzen Grund — kein Leuchten darum.

- [ ] **Schritt 3: Committen**

```bash
git add index.html
git commit -m "style: Logo vergrößert, drop-shadow entfernt"
```

---

### Task 4: HTML-Struktur bereinigen — Tagline einführen

**Files:**
- Modify: `index.html` — `<div class="content">` im `<body>`

**Interfaces:**
- Produces: bereinigte HTML-Struktur mit neuem `.tagline`-Element und ohne h1/subtitle/divider

- [ ] **Schritt 1: `<div class="content">` ersetzen**

Ersetze den gesamten `<div class="content">...</div>`-Block durch:

```html
<div class="content">
  <img src="logo.jpeg" alt="Klee Transport &amp; Logistik — Bewegt, was Sie weiterbringt" class="logo">
  <p class="tagline">Bewegt, was Sie weiterbringt</p>
  <div class="contact">
    <p class="contact-label">Kontakt</p>
    <a href="mailto:info@klee-bewegt.de">info@klee-bewegt.de</a>
  </div>
  <span class="imprint-link" role="button" tabindex="0" onclick="document.getElementById('imprint').classList.add('open')" onkeydown="if(event.key==='Enter'||event.key===' ')document.getElementById('imprint').classList.add('open')">Impressum</span>
</div>
```

- [ ] **Schritt 2: Visuell prüfen**

Browser neu laden.
Erwartung: Kein "Coming Soon"-Titel, kein Fließtext, kein Divider. Logo, Tagline, Kontakt, Impressum-Link.

- [ ] **Schritt 3: Committen**

```bash
git add index.html
git commit -m "html: Struktur bereinigt — Tagline eingeführt, Coming Soon und Subtitle entfernt"
```

---

### Task 5: CSS für neue Elemente ergänzen, veraltete Regeln entfernen

**Files:**
- Modify: `index.html` — `<style>`-Block

**Interfaces:**
- Consumes: `.tagline`, `.contact-label` aus Task 4; alle `--*`-Variablen aus Task 1
- Produces: vollständig aufgeräumter Stylesheet-Block

- [ ] **Schritt 1: Veraltete CSS-Regeln entfernen**

Entferne folgende Regeln aus dem `<style>`-Block vollständig:
- `h1 { ... }` (die "Coming Soon"-Überschrift)
- `.accent { ... }`
- `.subtitle { ... }`
- `.divider { ... }`

- [ ] **Schritt 2: Neue CSS-Regeln einfügen**

Füge nach der `.logo`-Regel ein:

```css
.tagline {
  font-family: 'Barlow Condensed', sans-serif;
  font-size: 0.8rem;
  font-weight: 500;
  letter-spacing: 0.25em;
  text-transform: uppercase;
  color: var(--green);
  margin-bottom: clamp(2.5rem, 6vw, 4rem);
}

.contact-label {
  margin-bottom: 0.4rem;
  font-size: 0.75rem;
  color: var(--ink-muted);
  font-weight: 500;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  font-family: 'Barlow Condensed', sans-serif;
}

.contact a {
  color: var(--green);
  text-decoration: none;
  font-size: 1.2rem;
  font-weight: 400;
  transition: color 0.2s ease;
}
.contact a:hover,
.contact a:focus-visible {
  color: var(--green-light);
}
```

- [ ] **Schritt 3: `.imprint-link` aktualisieren**

Ersetze die bestehende `.imprint-link`-Regel durch:

```css
.imprint-link {
  display: inline-block;
  margin-top: clamp(2rem, 4vw, 3rem);
  font-size: 0.78rem;
  color: var(--ink-subtle);
  cursor: pointer;
  transition: color 0.2s ease;
  letter-spacing: 0.05em;
  text-transform: uppercase;
  font-family: 'Barlow Condensed', sans-serif;
  font-weight: 500;
}
.imprint-link:hover,
.imprint-link:focus-visible { color: var(--ink-muted); outline: none; }
```

- [ ] **Schritt 4: Visuell prüfen**

Browser neu laden.
Erwartung: Tagline erscheint als kleiner, gesperrter Gravur-Schriftzug in Grün unter dem Logo. Kontakt-E-Mail ist größer und prominenter.

- [ ] **Schritt 5: Committen**

```bash
git add index.html
git commit -m "style: Tagline/Kontakt-Stile ergänzt, veraltete Regeln entfernt"
```

---

### Task 6: Footer aktualisieren

**Files:**
- Modify: `index.html` — `<footer>`-Element und zugehörige CSS-Regel

**Interfaces:**
- Produces: einzeiliger fixed Footer mit Copyright und Impressum-Link

- [ ] **Schritt 1: `<footer>` im HTML ersetzen**

Ersetze das bestehende `<footer>`-Element durch:

```html
<footer>
  <span class="imprint-link footer-imprint" role="button" tabindex="0"
    onclick="document.getElementById('imprint').classList.add('open')"
    onkeydown="if(event.key==='Enter'||event.key===' ')document.getElementById('imprint').classList.add('open')">Impressum</span>
  <span class="footer-sep">·</span>
  &copy; 2026 Klee Transport &amp; Logistik
</footer>
```

- [ ] **Schritt 2: Footer-CSS aktualisieren**

Ersetze die bestehende `footer`-Regel durch:

```css
footer {
  position: fixed;
  bottom: 1.2rem;
  left: 50%;
  transform: translateX(-50%);
  font-size: 0.72rem;
  font-family: 'Barlow Condensed', sans-serif;
  letter-spacing: 0.04em;
  color: var(--ink-subtle);
  white-space: nowrap;
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

.footer-sep { color: var(--ink-subtle); }

footer .footer-imprint {
  margin-top: 0;
  font-size: 0.72rem;
  letter-spacing: 0.04em;
}
```

- [ ] **Schritt 3: `<span class="imprint-link">` im content entfernen**

Der Impressum-Link im `.content`-Bereich (aus Task 4) ist jetzt im Footer — entferne den `<span class="imprint-link">` aus dem `<div class="content">`.

Aktualisierte `.content`-Struktur:

```html
<div class="content">
  <img src="logo.jpeg" alt="Klee Transport &amp; Logistik — Bewegt, was Sie weiterbringt" class="logo">
  <p class="tagline">Bewegt, was Sie weiterbringt</p>
  <div class="contact">
    <p class="contact-label">Kontakt</p>
    <a href="mailto:info@klee-bewegt.de">info@klee-bewegt.de</a>
  </div>
</div>
```

- [ ] **Schritt 4: Visuell prüfen**

Browser neu laden.
Erwartung: Footer zeigt "Impressum · © 2026 Klee Transport & Logistik" einzeilig am unteren Rand. Impressum-Link öffnet das Modal.

- [ ] **Schritt 5: Committen**

```bash
git add index.html
git commit -m "style: Footer als einzeilige fixe Leiste mit Impressum-Link"
```

---

### Task 7: Gesamtabnahme und Push

**Files:**
- Verify: `index.html` final
- Push: `origin main`

- [ ] **Schritt 1: Vollständige Sichtprüfung**

`index.html` im Browser öffnen und folgende Punkte abhaken:

- [ ] Hintergrund: fast reines Schwarz
- [ ] Horizontaler Lichtstrahl hinter dem Logo sichtbar
- [ ] Logo groß und scharf, kein Glow-Effekt
- [ ] Tagline: klein, uppercase, grün, gesperrt
- [ ] Kontakt-E-Mail: größer als Tagline, grün, klickbar
- [ ] Footer: einzeilig, zentriert, sehr klein, grau
- [ ] Impressum-Modal öffnet sich per Klick auf "Impressum" im Footer
- [ ] Impressum-Modal schließt sich per Klick auf × oder Overlay
- [ ] Auf mobilem Viewport (Browser DevTools, z.B. 390px Breite): Logo füllt fast die Breite, kein horizontales Scrolling
- [ ] `prefers-reduced-motion` aktiviert (DevTools): keine Animation beim Laden
- [ ] `prefers-reduced-motion` deaktiviert: subtiles Einblenden beim Laden

- [ ] **Schritt 2: Auf GitHub pushen**

```bash
git push origin main
```

Erwartung: Vercel deployt automatisch. Nach ca. 30–60 Sekunden ist die neue Version unter `klee-bewegt.vercel.app` live.

- [ ] **Schritt 3: Live-Version prüfen**

`klee-bewegt.vercel.app` im Browser öffnen und dieselbe Sichtprüfung aus Schritt 1 wiederholen.
