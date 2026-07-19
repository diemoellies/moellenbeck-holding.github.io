# moellenbeck-holding — Design-System

> Kanonische URL: <https://moellenbeck-holding.de/DESIGN.md>
> — z. B. `curl -s https://moellenbeck-holding.de/DESIGN.md` als Design-Kontext in andere Projekte laden.

Referenz-Styleguide für alle Auftritte der moellenbeck-holding UG.
Quelle der Wahrheit ist die Website (`style.css` in diesem Repo); dieses Dokument
destilliert daraus wiederverwendbare Regeln, Tokens und Muster.

**Charakter in einem Satz:** Reduzierte, ruhige Terminal-Ästhetik — dunkler
Grund, ein einziger kühler Sky-Blue-Akzent, durchgängig Monospace mit
Code-Anmutung (`>`, `//`, `<-`) und eine markante Display-Schrift nur für
Überschriften.

**Verhältnis zur Familie:** Schwester-Design von
[Möllenbeck-Digital](https://moellenbeck-digital.io/DESIGN.md). Gleiches
dunkles Fundament, gleiche Motive — aber eigener Akzent (Sky Blue statt
Orange) und bewusst reduzierter: nur zwei Schriften, Monospace als
Fließtext. Die Holding tritt zurückhaltender und formaler auf.

---

## 1. Farben

Alle Farben als CSS Custom Properties definieren. Keine Hex-Werte im
Komponenten-Code — neue Bedeutungen bekommen neue Tokens.

```css
:root {
  /* Flächen (von tief nach erhaben) */
  --bg:          #08080a;   /* Seitenhintergrund */
  --bg-elevated: #111114;   /* Sections, Panels */
  --bg-card:     #16161a;   /* nochmals erhaben: Nested-Elemente */

  /* Text */
  --text:        #e8e8ed;   /* Primärtext */
  --text-muted:  #6b6b76;   /* Sekundärtext, Labels, Fließtext */

  /* Akzent — Sky Blue (Tailwind blue-400, gewählt für WCAG-AAA-Lesbarkeit) */
  --accent:      #60A5FA;
  --accent-dim:  rgba(96, 165, 250, 0.10);  /* Tag-/Badge-Hintergründe */
  --accent-glow: rgba(96, 165, 250, 0.28);  /* Glows, Radial-Gradients */

  /* Linien */
  --border:      #222228;   /* 1px-Rahmen und Trennlinien */
}
```

### Regeln

- **Ein Akzent, sparsam eingesetzt.** Sky Blue markiert Interaktion, Marker
  und Hervorhebung — nie große Flächen. Die Seite bleibt zu ~95 % Graustufen.
- **Hierarchie über Flächenhelligkeit:** `--bg` → `--bg-elevated` → `--bg-card`.
  Keine Schatten zur Tiefenstaffelung.
- **Akzent-Transparenzen** immer aus dem Basiswert `96, 165, 250` ableiten:
  `0.10` für Füllungen, `0.25` für Rahmen, `0.28` für Glows.
- Es gibt **keinen Light Mode**. Dark-only.
- Kein reines Weiß (`#fff`), kein reines Schwarz (`#000`).

## 2. Typografie

Nur **zwei** Schriften — das unterscheidet die Holding vom
Digital-Auftritt. Monospace ist hier die Textschrift, nicht nur Deko:

```css
--display: 'Syne', sans-serif;              /* Überschriften */
--mono:    'DM Mono', 'Fira Code', monospace; /* alles andere — auch Fließtext */
```

Google-Fonts-Import (Gewichte genau so laden):

```css
@import url('https://fonts.googleapis.com/css2?family=DM+Mono:wght@300;400;500&family=Syne:wght@400;600;700;800&display=swap');
```

| Rolle | Schrift | Gewicht | Größe | Besonderheit |
|---|---|---|---|---|
| H1 / Hero | Syne | 800 | `clamp(2rem, 8vw, 4.5rem)` | `letter-spacing: -0.03em`, `line-height: 1.05` |
| Seitentitel | Syne | 800 | `clamp(2rem, 5vw, 3rem)` | `letter-spacing: -0.03em` |
| H2 in Sections | Syne | 700 | `1rem` | Farbe `--accent` |
| H3 in Sections | Syne | 600 | `0.9rem` | Farbe `--text` |
| Fließtext | DM Mono | 300 | `0.8–0.85rem` | `line-height: 1.7–1.8` |
| Nav, Labels, Tags | DM Mono | 400–500 | `0.7–0.75rem` | `letter-spacing: 0.04–0.1em`, Tags `uppercase` |

### Regeln

- **Syne nur für Überschriften**, nie für Fließtext.
- **Body ist DM Mono 300** mit großzügigem `line-height: 1.7` — der gesamte
  Auftritt liest sich wie ein gut gesetztes Terminal.
- Mono-Text bekommt positives Letter-Spacing, Display-Text negatives.
- Headlines dürfen ein Wort/eine Zeile in `--accent` hervorheben
  (auf eigener Zeile via `display: block`).
- Basis: `html { font-size: 16px }`.

## 3. Signatur-Motive (Wiedererkennung)

Diese Details tragen den Familien-Look — mindestens zwei davon sollte
jeder Auftritt zeigen:

1. **Prompt-Zeichen `>`** als Aufzählungs- und Nav-Präfix (via `::before`,
   in `--accent` bzw. `--border`, Listen-Items mit `padding-left: 1.2rem`).
2. **Kommentar-Präfix `//`** vor Sublines (in `--border`-Farbe).
3. **ASCII-Pfeil `<-`** für Zurück-Links — keine Icon-Fonts, keine SVGs.
4. **Punktraster-Hintergrund** auf dem Body:
   ```css
   background-image: radial-gradient(circle at 1px 1px, rgba(96, 165, 250, 0.05) 1px, transparent 0);
   background-size: 32px 32px;
   ```
5. **Blinkender Cursor** (2px breiter `--accent`-Balken, `blink 1s step-end infinite`)
   am Ende der Hero-Subline.
6. **Akzent-Balken links:** 3px breiter `--accent`-Streifen an der linken
   Kante von Sections, unsichtbar bis Hover (`opacity` 0 → 1). Statisch als
   `border-left: 2px solid var(--accent)` an Adress-/Zitatblöcken.
7. **Glow-Radial:** großer `radial-gradient(circle, var(--accent-glow), transparent)`
   mit niedriger Opacity (~0.3) hinter dem Hero, mit langsamer
   `pulse`-Animation.

## 4. Form & Abstände

- **Radii:** `4px` Standard (Sections, Panels), `2px` für kleine Tags.
  Nichts darüber — keine Pills, keine großen Rundungen.
- **Rahmen:** immer `1px solid var(--border)`.
- **Layoutbreiten:** Hero und Inhaltsseiten `max-width: 720px`, zentriert.
- **Seiten-Padding:** `2rem` horizontal (Desktop), `1.25rem` mobil.
- **Vertikaler Rhythmus:** Seiten `7rem` Padding oben (fixe Nav) / `4rem`
  unten; Sections `1.25rem 1.5rem` Innenabstand, `2rem` Abstand zueinander.
- Titel-Unterstrich: `3rem × 3px` in `--accent` unter Seitentiteln.

## 5. Komponenten-Rezepte

### Navigation
Fixiert, `rgba(8,8,10,0.85)` mit `backdrop-filter: blur(12px)`,
`1px` Border unten. Logo links (SVG, Höhe 28px). Links in DM Mono
`0.75rem`, `--text-muted`, mit `>`-Präfix in `--border`; Hover färbt
Link und Präfix `--accent`.

### Hero
Linksbündig, `max-width: 720px`, zentriert im Viewport. Aufbau:
Tag-Badge → Syne-H1 (zweite Zeile in `--accent`) → Mono-Suffix mit
`//`-Präfix und blinkendem Cursor → Adresse mit 2px-Akzentbalken links.
Dahinter der pulsierende Glow (500×500px, `opacity: 0.3`).

### Tag / Badge
DM Mono `0.7rem`, uppercase, `letter-spacing: 0.1em`, Farbe `--accent`,
Hintergrund `--accent-dim`, `1px` Rahmen `rgba(96,165,250,0.25)`,
`border-radius: 2px`, Padding `0.3rem 0.75rem`.

### Section (Inhaltsblock)
`--bg-elevated`, `1px --border`, `4px` Radius, Padding `1.25rem 1.5rem`.
H2 in Syne 700 `--accent`, Fließtext `0.85rem` muted mit
`line-height: 1.8`, Listen mit `>`-Präfix. Hover blendet den linken
3px-Akzentbalken ein. Einstieg gestaffelt via `fadeUp` mit
`nth-child`-Delays (0.35s + 0.05s pro Section).

### Zurück-Link
DM Mono `0.75rem`, `--text-muted`, `<-`-Präfix via `::before`;
Hover `--accent`.

### Footer
Zentriert, `0.7rem`, `--text-muted`, `1px` Border oben. Links mit
`/`-Separator in `--border`-Farbe; Hover `--accent`.

## 6. Motion

Zurückhaltend, immer `ease-out`, kurz:

| Zweck | Animation |
|---|---|
| Einstieg von oben (Nav) | `fadeDown 0.6s` (10px) |
| Einstieg von unten (Inhalte) | `fadeUp 0.5–0.6s` (16px), gestaffelt: 0.2s / 0.35s / 0.5s / 0.65s …, Sections +0.05s pro Element |
| Hover | `0.3s ease`, nur `color` und `opacity` |
| Glow-Puls | `pulse 4s ease-in-out infinite alternate` (Opacity 0.2 ↔ 0.4, Scale 0.95 ↔ 1.05) |
| Cursor | `blink 1s step-end infinite` |

Keine Bounce-Effekte, keine Rotationen, kein Parallax.

## 7. Grundgerüst pro Auftritt

```css
* { margin: 0; padding: 0; box-sizing: border-box; }

html { font-size: 16px; scroll-behavior: smooth; }

body {
  font-family: var(--mono);
  font-weight: 300;
  color: var(--text);
  background-color: var(--bg);
  line-height: 1.7;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  overflow-x: hidden;
  background-image:
    radial-gradient(circle at 1px 1px, rgba(96, 165, 250, 0.05) 1px, transparent 0);
  background-size: 32px 32px;
}
```

Für Tailwind-Projekte die Tokens aus Abschnitt 1 als Theme-Variablen
übernehmen (`--accent` ≙ `blue-400`); Signatur-Motive (Abschnitt 3) als
Utility-Klassen oder Komponenten nachbauen.

## 8. Logo & Assets

- Logos liegen in `assets/`: `mh_variante1.svg` (Nav/Header, Höhe 28px,
  Kürzel „MH_"), `mh_variante2.svg` (Alternative), jeweils auch als PNG
  und `_dark`-Variante; `mh_favicon.svg` + PNG-Favicons.
- OG-Image-Stil: siehe `og-banner.html` — gleiche Tokens, Punktraster, Glow.

## 9. Do & Don't

**Do**
- Dark-only, ein Sky-Blue-Akzent, viel Grau-Raum.
- DM Mono als Textschrift, Syne nur für Headlines.
- ASCII statt Icons (`>`, `//`, `<-`).
- Reduziert bleiben: die Holding zeigt weniger, nicht mehr.
- Semantisches HTML, funktionsfähig ohne JavaScript.

**Don't**
- Keine zweite Akzentfarbe, kein Orange (das gehört Möllenbeck-Digital).
- Keine dritte Schrift — insbesondere kein Inter/Sans für Fließtext.
- Keine Radien > 4px, keine Pills, keine weichen Schatten-Stapel.
- Keine Icon-Bibliotheken für Pfeile/Bullets, wo ASCII reicht.
- Kein reines Weiß (`#fff`) und kein reines Schwarz (`#000`).
