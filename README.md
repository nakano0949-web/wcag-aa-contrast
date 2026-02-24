# wcag-aa-contrast
A technical proposal addressing anti-aliasing–induced contrast loss in WCAG evaluations.
---

Anti‑Aliasing Contrast Model for WCAG
A proposal for addressing anti‑aliasing–induced contrast loss in WCAG evaluations.

---

Overview

This repository contains the technical materials supporting a proposal submitted to the W3C WCAG Working Group.  
The proposal addresses a known but undocumented issue: anti‑aliasing significantly reduces measured contrast ratios, especially for small font sizes, even when CSS‑specified colors meet WCAG requirements.

The repository includes:

- A mathematical model (Core Ratio Model)  
- Empirical data across platforms  
- SVG figures illustrating rendering structure and contrast loss  
- An extraction algorithm for Core / AA / BG‑Ring regions  
- A complete technical document suitable for WCAG review  

---

Motivation

WCAG 2.x defines contrast using intended colors, assuming uniform rendering.  
Modern rendering engines introduce anti‑aliasing that blends text and background colors, causing:

- Measured contrast to be 30–60% lower than theoretical contrast  
- False failures in automated accessibility tools  
- Inconsistent results across platforms and browsers  

This repository provides a reproducible framework to quantify and address this discrepancy.

---

Contents

`
/figures
  figure1.svg   # Three-layer rendering model
  figure2.svg   # Core shrinkage at small font sizes
  figure3.svg   # Core Ratio model
  figure4.svg   # Font size vs AA impact
  figure5.svg   # CSS vs measured contrast difference
  figure6.svg   # Extraction algorithm flowchart

/appendix
  appendix.pdf  # Mathematical definitions, empirical data, algorithm, rendering notes

proposal.md     # Full technical document (normative + informative sections)
README.md       # This file
`

---

Core Concepts

Three‑Layer Rendering Model
- Core Region — pure text color  
- AA Band — blended intermediate colors  
- BG‑Ring — background sampling region  

Core Ratio Model
A mathematical model describing how much of a glyph remains unaffected by anti‑aliasing.

\[
\text{CoreRatio} = (C/W)^2
\]

Contrast Difference Thresholds
- >25% → measured contrast unreliable  
- <15% → measured contrast reliable  
- 15–25% → warning range  

---

WCAG Proposal

The full proposal submitted to the WCAG Working Group is available in:

`
proposal.md
`

It includes:

- Normative guidance  
- Informative background  
- Figures and mathematical definitions  
- Recommendations for WCAG 3  

---

License

This repository is released under the MIT License.  
You may reuse the models, figures, and algorithms with attribution.

---

Contact

For questions or discussion, please open an Issue or contact the author.

---
