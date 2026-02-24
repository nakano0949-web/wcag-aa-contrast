# wcag-aa-contrast

A technical proposal addressing anti-aliasing–induced contrast loss in WCAG evaluations.

---

## Overview

This repository contains the technical materials supporting a proposal submitted to the W3C WCAG Working Group.  
The proposal addresses a known but undocumented issue: anti‑aliasing significantly reduces measured contrast ratios, especially for small font sizes, even when CSS‑specified colors meet WCAG requirements.

The repository includes:

- A mathematical model (Core Ratio Model)  
- Empirical data across platforms  
- SVG figures illustrating rendering structure and contrast loss  
- An extraction algorithm for Core / AA / BG‑Ring regions  
- A complete technical document suitable for WCAG review  

---

## Motivation

WCAG 2.x defines contrast using intended colors, assuming uniform rendering.  
Modern rendering engines introduce anti‑aliasing that blends text and background colors, causing:

- Measured contrast to be 30–60% lower than theoretical contrast  
- False failures in automated accessibility tools  
- Inconsistent results across platforms and browsers  

This repository provides a reproducible framework to quantify and address this discrepancy.

---
## Contents

/figures
  1.svg   # Three-layer rendering model
  2.svg   # Core shrinkage at small font sizes
  3.svg   # Core Ratio model
  4.svg   # Font size vs AA impact
  5.svg   # CSS vs measured contrast difference
  6.svg   # Extraction algorithm flowchart

/appendix
  appendix.pdf  # Mathematical definitions, empirical data, algorithm, rendering notes

proposal.md     # Full technical document (normative + informative sections)
README.md       # This file
---

## Core Concepts

### Three-layer Rendering Model
- **Core Region** — pure text color  
- **AA Band** — blended intermediate colors  
- **BG-Ring** — background sampling region  

![1 — Three-layer rendering model](figures/figure1.svg)

---

### Core Shrinkage at Small Font Sizes
Anti-aliasing dominates at small font sizes, reducing the Core Region.

![2 — Core shrinkage at small font sizes](figures/figure2.svg)

---

### Core Ratio Model
A mathematical model representing the proportion of a glyph unaffected by anti-aliasing.

\[
\text{CoreRatio} = (C/W)^2
\]

![3 — Core Ratio model](figures/figure3.svg)

---

### Font Size vs Anti-Aliasing Impact

![4 — Font size vs AA impact](figures/figure4.svg)

---

### CSS vs Measured Contrast Difference

![5 — CSS vs measured contrast difference](figures/figure5.svg)

---

### Extraction Algorithm (Core / AA / BG-Ring)

![6 — Extraction algorithm flowchart](figures/figure6.svg)

---

## Contrast Difference Thresholds

- **>25%** → measured contrast unreliable  
- **<15%** → measured contrast reliable  
- **15–25%** → warning range  

---

## WCAG Proposal

The full proposal submitted to the WCAG Working Group is available in:        
