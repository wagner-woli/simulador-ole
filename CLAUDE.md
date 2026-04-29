# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Single-file HTML simulator for **Woli Mídia** — a commercial tool used by sales reps to generate media proposals for retail networks (Eletro Móveis / Indústria & Serviços). The simulator calculates campaign costs by network, package, duration, and number of weeks, then generates a branded PDF proposal.

- **One file only**: `index.html` (~1700 lines, no build system, no dependencies except CDN-loaded jsPDF and Google Fonts)
- **Run locally**: `python3 -m http.server 8080` then open `http://localhost:8080`
- **Deploy**: GitHub Pages (the file is served directly as `index.html`)

## Architecture

All logic lives inside `index.html` in two `<script>` blocks:

### Data Constants (lines ~749–781)

```js
const REDES    // retail network definitions
const PACKAGES // ad frequency packages (presença/conversão/liderança/ataque)
const DURATIONS // creative durations (30s/60s/90s/120s) with discount multipliers
const HORAS = 51 // hours per week block (Mon–Sat)
const LOGO_BASE64 // Woli Mídia logo embedded as base64 JPEG
```

### REDES Array — The Most Common Edit Target

Each entry controls one retail network:

```js
{ key:'zema', nome:'Zema', total:496, cor:'#1a3a8f', ativa:true, estados:['MG','SP','ES','GO','BA','MS'] }
```

- `ativa: true` → shows as active (user can set store count); `ativa: false` → shows "Em implantação" badge, input disabled
- `estados` → Brazilian state codes used for SVG map coloring and tooltip data
- `total` → max stores; limits the number input

**Current active networks**: Zema, Dujuca, MM Lojas, Colombo, Lojas Ramos  
**Currently in implantação**: Simonetti, Fujioka, Solar Magazine, Móveis Linhares, TV Multiloja, Valdar

### Key Functions

| Function | Purpose |
|---|---|
| `buildRedesGrid()` | Renders the 2-column network grid + SVG Brazil map |
| `buildPkgGrid()` | Renders frequency package selector buttons |
| `calc()` | Recalculates all metrics; called after every user interaction |
| `updateRede(el)` | Handles store count input changes |
| `setAllRede(key, total)` | "Todas" / "Limpar" buttons |
| `addCriativo(dur, preco)` | Adds a creative production item |
| `alterarCriativo(dur, delta)` | +/− on creative items |
| `renderCriativos()` | Re-renders the creative list |
| `openModal()` | Opens the PDF preview modal |
| `gerarPDF()` | Generates and downloads the PDF via jsPDF |

### Initialization (bottom of file, lines ~1383–1385)

```js
buildRedesGrid();
initChart();
calc();
```

No `DOMContentLoaded` wrapper — scripts run after the HTML body.

### Pricing Model

- **Campaign cost**: `lojas × price_per_store × blocos × (1 - discount)`
  - `price_per_store` comes from the selected `PACKAGES` entry (R$40/80/120/160)
  - `blocos` = weeks × duration multiplier
  - `discount` from `DURATIONS` (0%, 10%, 20%, 30% for 30s/60s/90s/120s)
- **Creative production** (optional): flat fee per creative (R$1.500/2.500/3.000/3.500)
- **Minimum**: 1 block (1 week)

### State (`redesSel` object)

Selected store counts are tracked in:
```js
const redesSel = {};  // { 'zema': 120, 'colombo': 0, ... }
```

## Important: Markdown Artifacts in Pasted HTML

If the user pastes this HTML via a chat interface (e.g., Claude.ai web), **the markdown renderer corrupts JavaScript and CSS**:

- JS property accesses like `r.total` → `[r.total](http://r.total)`
- CSS compound selectors like `.discount-pill.show` → `.[discount-pill.show](http://discount-pill.show)`

Before writing pasted HTML to disk, fix these with:
```python
import re
fixed = re.sub(r'\[([^\]]+)\]\(http://\1\)', r'\1', content)
# Then manually fix: ['PACOTE', [pkg.name](http://pkg.name) → ['PACOTE', pkg.name
```
