# Design Spec: Coming Soon Redesign — Klee Transport & Logistik

**Datum:** 2026-07-07  
**Ansatz:** Monumentale Stille  
**Zielgefühl:** Stärke und Verlässlichkeit

---

## Kontext

Die bestehende Coming-Soon-Seite (Barlow-Fonts, OKLCH-Farben, dunkles Theme, Impressum-Modal) wird in derselben Richtung weiterentwickelt — stärker, eindrucksvoller, reduktiver. Keine vollständige Neuausrichtung.

---

## Layout & Struktur

Vertikal und horizontal zentriert. Strenge, klare Hierarchie — alle nicht-essentiellen Elemente entfernt.

```
[viel Luft oben]
[LOGO — groß, ~92vw, max 600px]
[Tagline]
[viel Luft]
[Kontakt-Label + E-Mail]
[fixed footer: Impressum · Copyright]
```

**Entfernt gegenüber aktueller Version:**
- "Coming Soon"-Überschrift
- Fließtext ("Wir arbeiten an unserem neuen Webauftritt...")
- Divider-Strich (48px grüne Linie)

Das Logo ist die einzige visuelle Aussage. Der Weißraum um es herum ist Teil des Designs, nicht Leerstelle.

---

## Hintergrund & Licht

**Hintergrundfarbe:** `oklch(0.06 0 0)` — fast reines Schwarz, kühler und härter als der aktuelle leicht grünliche Ton.

**Lichteffekt:** Ein einzelner horizontaler Lichtstrahl hinter dem Logo. Umgesetzt als sehr schmales `radial-gradient` mit geringer vertikaler Streuung — grün, scharf, nicht diffus. Wirkt wie ein Scheinwerfer auf nasser Straße, nicht wie ein Glüh-Halo.

```css
background: radial-gradient(
  ellipse 80% 120px at 50% 38%,
  var(--green-glow) 0%,
  transparent 100%
);
```

**Logo-Behandlung:** Kein `drop-shadow`. Das Logo steht unverfälscht auf dem schwarzen Grund — Kontrast ersetzt den Glow-Effekt.

---

## Farbe

| Variable         | Wert                         | Verwendung                        |
|------------------|------------------------------|-----------------------------------|
| `--green`        | `oklch(0.58 0.18 145)`       | Akzentfarbe, Links, Tagline       |
| `--green-glow`   | `oklch(0.58 0.18 145 / 0.2)` | Hintergrund-Lichtstrahl           |
| `--ink`          | `oklch(0.93 0 0)`            | Kontakt-E-Mail                    |
| `--ink-muted`    | `oklch(0.55 0 0)`            | Kontakt-Label                     |
| `--ink-subtle`   | `oklch(0.38 0 0)`            | Footer                            |
| `--bg`           | `oklch(0.06 0 0)`            | Seitenhintergrund                 |
| `--bg-elevated`  | `oklch(0.12 0.005 145)`      | Impressum-Modal                   |
| `--border`       | `oklch(0.20 0.005 145)`      | Impressum-Modal-Rahmen            |

Grün gegenüber aktuell leicht kräftiger (Chroma 0.18 statt 0.15).

---

## Typografie

Schriften: **Barlow** + **Barlow Condensed** (Google Fonts) — unverändert.

### Tagline
- Text: `"Bewegt, was Sie weiterbringt"`
- Font: Barlow Condensed, weight 500
- Größe: `0.8rem`
- Transform: uppercase
- Letter-spacing: `0.25em`
- Farbe: `var(--green)` — gedämpft, wirkt wie Gravur

### Kontakt-Label
- Text: `"Kontakt"`
- Font: Barlow Condensed, weight 500
- Größe: `0.75rem`
- Transform: uppercase
- Letter-spacing: `0.1em`
- Farbe: `var(--ink-muted)`

### Kontakt-E-Mail
- Text: `info@klee-bewegt.de`
- Font: Barlow, weight 400
- Größe: `1.2rem`
- Farbe: `var(--green)` → `var(--green-light)` on hover

### Footer (fixed bottom)
- Text: `Impressum · © 2026 Klee Transport & Logistik`
- Font: Barlow Condensed, weight 400
- Größe: `0.72rem`
- Letter-spacing: `0.04em`
- Farbe: `var(--ink-subtle)`
- "Impressum" ist klickbar und öffnet das bestehende Modal

---

## Animation

Keine. Einzige Ausnahme: der bestehende `arrive`-Keyframe (opacity + translateY) bleibt für den initialen Seitenaufruf, aber nur unter `prefers-reduced-motion: no-preference`. Keine Loops, keine Pulse, keine Parallax.

---

## Impressum-Modal

Unverändert — Inhalt, Struktur und JavaScript bleiben identisch. Nur die CSS-Variablen werden auf die neuen Werte angepasst (`--bg-elevated`, `--border`).

---

## Was bleibt unverändert

- Semantisches HTML mit aria-labels
- `<meta name="description">` und viewport-Tag
- `lang="de"`
- `focus-visible`-Stile
- Responsive Sizing mit `clamp()`
- Google Fonts Preconnect

---

## Erfolgskriterium

Die Seite vermittelt beim ersten Blick Stärke und Verlässlichkeit — ohne dass der Besucher bewusst analysieren muss, warum. Das Logo dominiert. Die Stille um es herum ist keine Abwesenheit, sondern Aussage.
