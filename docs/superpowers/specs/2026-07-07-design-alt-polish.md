# Design Spec: design-alt Polish — Klee Transport & Logistik

**Datum:** 2026-07-07  
**Branch:** design-alt  
**Ausgangslage:** Photo-Hero-Design (LKW auf Autobahn bei Sonnenuntergang, Unsplash / Joshua Woroniecki)  
**Ziel:** Vier chirurgische Fixes — keine neuen Elemente, kein Struktureingriff

---

## Kontext

Das bestehende design-alt Layout (Logo oben, Eyebrow-Label, H1 "COMING SOON", Divider, Subtitle, Kontakt, Footer) ist konzeptionell stark und braucht keine Umbauten. Diese Spec definiert vier präzise Verbesserungen, die das Design präsentationsreif machen.

---

## Fix 1 — Logo-Rahmen integrieren

**Problem:** `logo.jpeg` hat einen schwarzen Hintergrund. Auf dem Foto wirkt er als harter Kasten.

**Lösung:** CSS auf `.logo`:

```css
border-radius: 10px;
box-shadow: 0 8px 32px oklch(0 0 0 / 0.55);
```

**Ergebnis:** Der schwarze Kasten liest sich als bewusst gerahmte Karte. Die abgerundeten Ecken und der weiche Schatten lassen ihn sanft in den dunklen Hintergrund übergehen.

**Nicht tun:** `mix-blend-mode`, Padding-Hacks, PNG-Konvertierung — alle außerhalb des Scopes.

---

## Fix 2 — SOON-Typografie: Outline → Weiß

**Problem:** `h1 .word-soon` hat `color: transparent` + `-webkit-text-stroke: 2px var(--green)`. Das grüne Outline wirkt überladen neben dem Eyebrow-Label und dem Divider.

**Lösung:**

```css
h1 .word-soon {
  color: var(--ink);
  /* kein -webkit-text-stroke */
}
```

**Ergebnis:** Beide Wörter weiß gefüllt, lesbare saubere Headline. Der einzige grüne Farbakzent bleibt Eyebrow-Label und Divider — Grün wirkt dadurch gezielter.

---

## Fix 3 — Overlay-Gradient verstärken

**Problem:** Der Gradient-Overlay ist im Mittelbereich (55%) bei 0.70 Opacity. Bei hellem Bildmotiv kann der Text schlecht lesbar werden.

**Lösung:** Im `body` background-image:

```css
linear-gradient(
  to bottom,
  oklch(0.06 0 0 / 0.45) 0%,
  oklch(0.06 0 0 / 0.78) 55%,   /* war: 0.70 */
  oklch(0.06 0 0 / 0.88) 100%
)
```

**Ergebnis:** Text im Hauptbereich hat mehr Kontrast ohne das Foto zu erdrücken. Oben (Himmel) bleibt leicht transparent, unten (Straße) weiter dunkel.

---

## Fix 4 — Vertikale Abstände straffen

**Ziel:** Auf einem 1280×800-Desktop-Viewport soll der gesamte Inhalt (Logo bis Kontakt) ohne Scrollen sichtbar sein.

**Änderungen:**

| Element | Eigenschaft | Alt | Neu |
|---------|-------------|-----|-----|
| `.eyebrow` | `margin-bottom` | `clamp(0.75rem, 2vw, 1rem)` | `clamp(0.5rem, 1.5vw, 0.75rem)` |
| `h1` | `margin-bottom` | `clamp(1rem, 2.5vw, 1.75rem)` | `clamp(0.75rem, 2vw, 1.25rem)` |
| `.divider` | `margin-bottom` (bottom) | `clamp(1rem, 2vw, 1.5rem)` | `clamp(0.75rem, 1.5vw, 1rem)` |
| `.subtitle` | `margin-bottom` | `clamp(1.5rem, 3vw, 2rem)` | `clamp(1rem, 2vw, 1.5rem)` |
| `.site-header` | `padding-top/bottom` | `clamp(1.5rem, 4vw, 2.5rem)` | `clamp(1rem, 3vw, 1.75rem)` |

**Nicht ändern:** Schriftgrößen, Layout-Struktur, Footer-Abstände.

---

## Was unverändert bleibt

- HTML-Struktur komplett (kein Element hinzufügen oder entfernen)
- Impressum-Modal (Inhalt und JavaScript)
- Foto-URL und background-attachment: fixed
- Farbvariablen
- Schriften (Barlow, Barlow Condensed)
- Logo-Größe (`width: min(280px, 60vw)`)
- Footer-Layout

---

## Erfolgskriterium

Auf einem 1280×800-Desktop sieht man beim ersten Aufruf: Logo (gerahmt, weich), "COMING SOON" (beides weiß, groß, klar), grüner Divider, Subtitle-Text, Kontakt-E-Mail — alles ohne Scrollen. Das Foto ist sichtbar aber nicht ablenkend.
