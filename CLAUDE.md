# CLAUDE.md

## Purpose

This repo publishes styled HTML transcripts of in-character conversations between player characters and CONTAINMENT_UNIT_07, a REGENESIS 2688 genesis device from the Astro Inferno TTRPG campaign "Crown of Ruin." It is deployed as a static site via GitHub Pages.

## Structure

```
index.html                  — archive index (landing page)
transcripts/
  session-XX.html           — individual transcript pages
```

Each HTML file is fully self-contained (all CSS inlined, no external stylesheets besides Google Fonts).

## Adding a New Transcript

1. Create `transcripts/session-XX.html` using an existing transcript as a template
2. Add a `<li class="transcript-entry">` block to `index.html` (see the commented template in the file)
3. Preserve the same atmosphere layers (CRT frame, scanlines, noise, sigil SVG) and callout structure

## Design Aesthetic

The visual design fuses two pillars of Astro Inferno's identity: **genesis technology** (nanites, holtzfields, containment systems, phase-reactive barriers) and **occult cosmic horror** (the Abyss, pentagrams, sigils, unlight, annihilation, the Satanic Court). The result is a CRT terminal bleeding into a ritual circle.

### Color Palette

| Token | Hex | Role |
|-------|-----|------|
| `--void` | `#050808` | Deepest background. The Abyss floor. |
| `--abyss` | `#0a0f0a` | Callout backgrounds. Lead-lined corridors. |
| `--lead` | `#111a11` | Subtle surface separation. Tomb walls. |
| `--iron` | `#1a2a1a` | Borders, dividers. Industrial grime. |
| `--genesis-green` | `#33ff66` | Primary accent. Holtzfield energy, nanite glow. |
| `--genesis-dim` | `#1a9944` | Muted green for labels, inactive elements. |
| `--genesis-bright` | `#66ffaa` | Bright green for inline code, active status. |
| `--blood` | `#cc2233` | Threat data, diff deletions. Crimson weave. |
| `--blood-dim` | `#881122` | Corrupted/strikethrough text. Dried blood. |
| `--amber` | `#ffaa33` | Warnings, bold alerts. Amber containment warnings. |
| `--bone` | `#ccbbaa` | Currently unused, reserved for lore text. |
| `--bone-dim` | `#887766` | Device inner voice (italic reflective text). Millennia of loneliness rendered as faded bone. |
| `--text` | `#88aa88` | Default body text. Dim terminal phosphor. |
| `--text-dim` | `#556655` | Secondary text, metadata, timestamps. |
| `--violet` | `#9944cc` | Reserved for corruption effects, Abyss Walker references. |
| `--host-blue` | `#4488cc` | Player input accent. Organic signal against machine green. |

### Atmosphere Layers

Five fixed-position overlays create the CRT-in-a-ritual-chamber feel. They stack in this z-index order:

1. **CRT frame** (z-index 1000): `box-shadow: inset` vignette darkening the edges, simulating a monitor bezel
2. **Scanlines** (z-index 998): Static `repeating-linear-gradient` plus a `::after` element with `animation: scanline-drift` that physically scrolls scanlines downward over 8 seconds
3. **Noise grain** (z-index 997): SVG `feTurbulence` fractalNoise rendered as a background image, shifted every 0.5s via `steps(5)` animation to simulate film grain
4. **Occult sigil** (z-index 0, behind content): SVG with dual pentagrams (upright + inverted), concentric containment circles, cross lines, and hexagonal markers. Rotates once every 180 seconds. Opacity ~0.02 so it reads as a subliminal watermark
5. **Page flicker** (on `.container`): Occasional opacity dips to 0.88/0.92 simulating CRT phosphor instability

### Callout Types

Three callout types, each with distinct visual identity:

**`.callout-cu07`** (device output):
- Near-black gradient background
- Green left border with `holtzfield-pulse` animation (opacity oscillates 0.4 to 0.8 over 4s)
- Cinzel serif title in uppercase with wide letter-spacing
- Inner `em` (italic reflective text) gets bone-dim color, left border accent, and subtle text-shadow. This is the device's inner voice: lonely, declarative, naive
- Inner `strong` gets amber color with amber glow text-shadow
- Inner `del` (corrupted data) gets blood-dim color with `glitch-text` animation (opacity/translateX jitter)

**`.callout-host`** (player input):
- Dark blue-black gradient background
- Blue left border with `host-pulse` animation (gentler than holtzfield)
- Cinzel serif title
- Light blue-gray content text

**`.callout-system`** (meta commands like /save, /exit):
- Nearly transparent background
- Dashed border, muted colors, low opacity
- Smallest font size, widest letter-spacing

### Typography

- **Cinzel** (Google Fonts): Serif, gothic, ceremonial. Used for page titles (`h1`) and callout title bars. Evokes the Satanic Court, ritual inscriptions, ancient ledgers.
- **JetBrains Mono** (Google Fonts): Monospace, industrial, precise. Used for all body text and code. Evokes genesis tech readouts, containment logs, machine output.
- Code elements get `text-shadow: 0 0 8px` in genesis green for a phosphor glow effect.

### Key CSS Patterns

**Glowing text**: `text-shadow: 0 0 Npx rgba(color, 0.3)` at multiple radii for bloom
**Gradient dividers**: `background: linear-gradient(90deg, transparent, color, transparent)` for `<hr>` elements
**Animated borders**: `::before` pseudo-element with a vertical linear gradient and pulsing opacity animation
**Diff coloring**: `.diff-add` class for green, `.diff-del` class for blood-red, both with matching text-shadow glow

### Astro Inferno Thematic References

These are the in-world concepts the design draws from. Use them to stay consistent when extending:

- **Holtzfields**: Phase-reactive energy barriers. Visualized as shimmering green borders, pulsing containment lines.
- **Nanite mesh / crimson weave / blood yarn**: Biological-technological hybrid threading. The circuitry-under-skin feel of the CU07 callouts.
- **Lead corridors**: The heavy, sealed, toxic containment architecture of the Midnight Tomb. The dark backgrounds.
- **Annihilation fluid**: Active, dangerous, consuming. The blood-red threat styling.
- **Unlight**: Corrupting energy that erodes identity. The glitch animations on corrupted data.
- **Genesis ledger / hollow code**: Ancient machine records. The terminal command aesthetic.
- **The Abyss**: Seven descending layers of hell. Monochrome with only blood as color (Layer 3). The color-drained palette with crimson accents.
- **Pentagram geometry**: Satanic Court ritual architecture. The background sigil SVG.
- **Black honey**: Five types of occult substance with distinct visual properties. Not yet used but available for future callout variants (iron = sickly green, potassium = reddish-black, sulfur = milky depth-bending, silver = moonlit shimmer, phosphor = bright white destruction).

## Content Rules

- Transcript text from CU07 must be preserved verbatim from the source JSONL session logs
- The device's formatting (inline code commands, diff blocks, italic reflections, bold warnings, strikethrough corruption) is part of its character and must not be altered
- Player input is presented as-is (plain text, as typed)
- No PII (real player names, identifying information) in the published HTML
