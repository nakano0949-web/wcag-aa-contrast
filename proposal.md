# Proposal: Addressing Anti-Aliasing–Induced Contrast Loss in WCAG Evaluations

## 1. Introduction
Modern text rendering engines apply anti-aliasing (AA) to improve visual smoothness.  
However, AA introduces blended intermediate colors that significantly reduce the measured contrast ratio of text, especially at small font sizes.

This proposal documents the issue, provides empirical evidence, and recommends updates to WCAG evaluation methods.

---

## 2. Problem Statement
WCAG 2.x defines contrast using intended CSS colors, assuming uniform rendering.  
In practice, anti-aliasing blends text and background colors, causing:

- Measured contrast to be 30–60% lower than theoretical contrast  
- False failures in automated accessibility tools  
- Inconsistent results across platforms and browsers  

This discrepancy is not documented in WCAG 2.x or WCAG 3 drafts.

---

## 3. Three-Layer Rendering Model
We model rendered glyphs as three regions:

1. **Core Region** — pure text color  
2. **AA Band** — blended intermediate colors  
3. **BG-Ring** — background sampling region  

This structure is consistent across major platforms (Windows, macOS, iOS, Android).

---

## 4. Core Ratio Model
We define the proportion of a glyph unaffected by AA as:

$$
\text{CoreRatio} = (C/W)^2
$$

Where:
- \( C \) = core width  
- \( W \) = total glyph width  

This model predicts contrast loss as font size decreases.

---

## 5. Empirical Findings
Across platforms:

- AA reduces measured contrast by **30–60%**  
- Small fonts (< 14px) show the largest loss  
- CSS colors that pass WCAG AA often fail when measured from rendered pixels  

Figures 1–6 in the repository illustrate these findings.

---

## 6. Proposed WCAG Updates

### 6.1 Define AA-Induced Contrast Loss
WCAG should explicitly acknowledge that anti-aliasing reduces measurable contrast.

### 6.2 Introduce Contrast Difference Thresholds
We propose:

- **>25% difference** → measured contrast unreliable  
- **<15% difference** → measured contrast reliable  
- **15–25%** → warning range  

### 6.3 Recommend Core-Based Contrast Evaluation
Tools should measure contrast using the **Core Region**, not AA-blended pixels.

---

## 7. Conclusion
Anti-aliasing is a universal rendering behavior that significantly affects contrast measurements.  
This proposal provides a reproducible framework and mathematical model to address the issue within WCAG.

---

## 8. References
See repository figures and appendix for data, algorithms, and rendering notes.

---

Appendix A — Rendering Engine Notes

This appendix summarizes platform‑specific rendering behaviors relevant to anti‑aliasing (AA) and contrast evaluation.

A.1 Windows (DirectWrite)
- Subpixel AA enabled by default  
- Gamma correction applied before compositing  
- Core region is typically 1–2 px narrower than glyph outline  
- AA band width increases at small font sizes (< 14 px)

A.2 macOS / iOS (CoreGraphics)
- Uses grayscale AA by default  
- Stronger smoothing produces wider AA bands  
- Core region shrinkage is more pronounced than on Windows  
- Background sampling ring is stable across zoom levels

A.3 Android (Skia)
- Subpixel AA varies by device and OEM  
- Some devices apply post‑AA sharpening  
- AA band width is inconsistent across densities (mdpi → xxxhdpi)

A.4 Browsers
| Browser | Rendering Backend | Notes |
|--------|-------------------|-------|
| Chrome | Skia | Consistent AA band; predictable blending curve |
| Firefox | Skia / DirectWrite | Platform‑dependent; contrast varies by OS |
| Safari | CoreGraphics | Strong smoothing; largest AA‑induced contrast loss |

---

Appendix B — Core / AA / BG‑Ring Extraction Algorithm

This appendix describes the algorithm used to extract the three rendering regions from rasterized text.

B.1 Overview
The algorithm identifies:

1. Core Region — pixels matching the intended text color  
2. AA Band — blended pixels between text and background  
3. BG‑Ring — background sampling region surrounding the glyph  

B.2 Steps

Step 1 — Input
- Rasterized glyph image  
- Intended text color (RGB)  
- Intended background color (RGB)

Step 2 — Compute Color Distance
For each pixel \( p \):

$$
d(p) = \| p - C_{\text{text}} \|
$$

Pixels with minimal distance are candidates for the Core Region.

Step 3 — Thresholding
Define thresholds:

$T_{\text{core}}$ — maximum distance for core pixels  
$T_{\text{bg}}$ — minimum distance for background pixels  
Pixels between these thresholds form the AA Band.


Step 4 — Region Labeling
Core Region = connected components of core pixels  
AA Band = all pixels between $T_{\text{core}}$ and $T_{\text{bg}}$  
BG‑Ring = background pixels adjacent to the AA Band

Step 5 — Core Width Measurement
Compute:

$$
C = \text{mean core width across glyph}
$$

$$
W = \text{mean total glyph width}
$$

Step 6 — Core Ratio

$$
\text{CoreRatio} = (C/W)^2
$$

---

Appendix C — Empirical Measurement Protocol

This appendix documents the measurement procedure used to collect contrast data.

C.1 Test Matrix
Measurements were taken across:

- 4 platforms (Windows, macOS, iOS, Android)  
- 4 browsers (Chrome, Firefox, Safari, Edge)  
- 6 font sizes (10, 12, 14, 16, 18, 24 px)  
- 3 font weights (Regular, Medium, Bold)  
- 12 color pairs (WCAG‑passing and borderline cases)

C.2 Procedure
1. Render text using native browser engine  
2. Capture raster output at device pixel resolution  
3. Extract Core / AA / BG‑Ring regions  
4. Compute:
   - Theoretical contrast  
   - Measured contrast (min pixel contrast)  
   - Contrast difference percentage  
5. Repeat 10× per configuration  
6. Average results and compute variance

C.3 Reproducibility
All scripts, datasets, and figures are included in the repository.

---

Appendix D — Contrast Difference Threshold Justification

This appendix explains how the proposed thresholds were derived.

D.1 Observed Ranges
Across all platforms:

- AA‑induced contrast loss ranged from 18% to 61%  
- Core‑based contrast remained within 5% of theoretical values

D.2 Threshold Rationale
- >25% difference  
  - AA dominates pixel sampling  
  - Measured contrast no longer reflects intended design  
- 15–25% difference  
  - Transitional zone  
  - AA affects measurement but not severely  
- <15% difference  
  - Core region dominates  
  - Measured contrast is reliable

D.3 Alignment with WCAG Goals
These thresholds support:

- Interoperability  
- Predictability  
- Author intent preservation  
- Tool consistency  

---

Appendix E — Limitations and Future Work

E.1 Limitations
- Subpixel AA varies by device  
- Some OEMs apply undocumented post‑processing  
- Font hinting can alter core width  
- High‑DPI displays reduce AA impact but do not eliminate it

E.2 Future Work
- Extend model to variable fonts  
- Evaluate impact of color profiles  
- Explore perceptual weighting of AA pixels  
- Integrate with WCAG 3 “Perceptual Contrast” workstreams  

