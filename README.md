# Companion to *Introduction to Digital Filters* by Julius O. Smith III

An interactive, animated browser-native companion to Julius O. Smith III's
[*Introduction to Digital Filters with Audio Applications*](https://ccrma.stanford.edu/~jos/filters/)
— the W3K Publishing volume freely hosted at ccrma.stanford.edu. The book is the
canonical Stanford CCRMA text on filter design, the z-transform, and the geometry
of the unit circle; this page is a companion set of interactive experiments around
exactly those ideas.

**[&rarr; View the Live Guide](https://brendanjameslynskey.github.io/Companion_JOS_Introduction_to_Digital_Filters/)**

---

## What's Inside

Twelve chapters walk through the history, mathematics, and audio applications of
digital filters, with real-time interactive visualisations and audio playback
throughout.

### 1 &middot; Prologue &mdash; Why Digital Filters?
The difference equation $y[n] = \sum b_i x[n-i] - \sum a_j y[n-j]$, JOS's conventions,
and a pole-pair animation showing how a complex-conjugate pole pair becomes a
decaying sinusoid impulse response. Interactive slider over pole radius and angle.

### 2 &middot; A Sketch of the History
Illustrated timeline: Black's 1927 feedback amplifier; Nyquist / Kotelnikov /
Shannon sampling theorem; Bode 1945; Zadeh &amp; Ragazzini's 1952 z-transform;
Cooley &amp; Tukey FFT 1965; Kaiser 1966; Rader 1968; Gold &amp; Rader 1969 textbook;
Parks &amp; McClellan 1972; Rabiner &amp; Gold and Oppenheim &amp; Schafer 1975;
Bristow-Johnson's 1985 EQ cookbook; Proakis &amp; Manolakis 1988; through to the
modern plugin era.

### 3 &middot; Signals, Systems, LTI
Impulse response, convolution, causality, stability. Interactive convolution
visualiser: pick input (impulse, step, sine, square, noise) and kernel (smoother,
differencer, moving average, Hann LP, edge detector) and watch the kernel slide
under $x[n]$ while $y[n]$ builds up sample by sample.

### 4 &middot; The z-Transform &mdash; Flagship Interactive
Definition, ROC, the rational transfer function. Then the page's centrepiece:
click to place a zero, shift-click to place a pole, drag to move, double-click to
delete. Complex-conjugate pairs auto-added for real coefficients. Right panels
show $|H(e^{j\omega})|$ (dB), unwrapped phase, group delay, and impulse response;
the difference-equation coefficients are printed live. Presets for two-pole
resonator, notch, and a Butterworth-like cascade.

### 5 &middot; Frequency Response, Phase, Group Delay
Derivation of $H(e^{j\omega})$ from $H(z)$; group delay as $-d\phi/d\omega$.
Triple-panel interactive (mag / phase / group delay) for five representative
filters including a Hann-windowed FIR LP and an RBJ peaking biquad. Bode aside.

### 6 &middot; Canonical Elementary Filters
One-pole LP, one-zero LP, two-pole resonator, notch, peaking and shelving
biquads (Bristow-Johnson cookbook formulae). Four-panel interactive explorer:
pole-zero layout, magnitude response, impulse response, live coefficients
$\{b_i\}, \{a_j\}$.

### 7 &middot; Poles, Zeros, Stability, Minimum Phase
Stability &equiv; all poles strictly inside the unit circle. Minimum-phase /
allpass factorisation. Interactive: reflect a zero across the unit circle;
magnitude response is invariant but phase changes. Forward-reference to the
[Minimum / Maximum Phase Filters](https://github.com/BrendanJamesLynskey/MinimumMaximumPhaseFilters)
guide.

### 8 &middot; Implementation Structures
Direct Form I, Direct Form II, Transposed Direct Form II, cascade. Interactive
signal-flow diagrams (summers, multipliers, $z^{-1}$ delays) with a selector to
compare structures side by side.

### 9 &middot; FIR vs IIR Filter Design
Window-method FIR (rectangular, Hann, Hamming, Blackman); bilinear-transform
IIR Butterworth; a working Parks-McClellan / Remez-style equiripple FIR
(iteratively-reweighted least squares — a simplification of the 1972
algorithm); Kaiser window $(\beta, L)$ closed-form spec formulae. Live designer
with type (LP/HP/BP/BS), cutoff, bandwidth, order and window controls; plots
mag (dB) with passband / stopband edges marked, stem impulse response, and
group delay.

### 10 &middot; Allpass Filters
$H_{\text{ap}}(z) = z^{-N}A(z^{-1})/A(z)$; fractional delay; Schroeder reverb.
Interactive cascaded-allpass demo in a 2&times;2 grid: $|H|$ clamped to &plusmn;2&nbsp;dB
(the verification that it really is flat), unwrapped phase, group delay (what
an allpass actually _does_), and the diffusing impulse response.

### 11 &middot; Applications
Audible three-band peaking EQ on a synthesised kick+snare+hats drum loop, with
live coefficient update and an A/B "play original / play EQ'd" pair routed via
the Web Audio API. DC blocker and anti-alias / interpolation filter
discussions.

### 12 &middot; Legacy and Continuing Impact
Where digital filters live today: DAW plugins, converter decimation / interpolation,
oversampled nonlinear processors, warped IIRs, differentiable DSP. Full bibliography
and cross-links to sibling companion guides (Mathematics of the DFT, Spectral,
Physical, Faust) and adjacent guides (STFT, Laplace, Minimum/Maximum Phase,
Kramers-Kronig, Sound Transforms History).

---

## Technical Details

* **Zero dependencies.** No npm, no bundler, no external libraries. All rendering
  uses the HTML Canvas API. Audio uses the Web Audio API. Math typesetting uses
  KaTeX via jsdelivr &mdash; the only third-party asset.
* **All maths computed in real time.** Every frequency response, impulse response,
  group delay, pole-zero computation, FIR design, and biquad cascade is calculated
  live in the browser with hand-written routines. The radix-2 FFT and polynomial
  root-building utilities are written in plain JavaScript with no library.
* **Audio playback.** The drum loop is synthesised on demand (kick via exponentially
  swept sine, snare via filtered noise + body tone, hats via decayed noise) and
  routed through the three-biquad EQ cascade on every A/B click.
* **Responsive.** Canvases scale with DPR for sharp rendering on Retina / HiDPI
  displays.
* **Single file.** Everything lives in `index.html`.

### What's computed where

| Visualisation | Algorithm | Notes |
|---|---|---|
| Hero animation | Pole-pair on z-plane + matching $r^n \cos(\omega_0 n)$ IR | Animated orbit |
| Prologue mini demo | 2-pole resonator, live $r, \omega_0$ slider | z-plane, IR, $|H|$ |
| Convolution viz | Direct convolution sum; flipped-shifted kernel overlay | Auto-slide available |
| Flagship z-plane | Draggable poles/zeros; `freqz`, `groupDelay`, `impulseResponse` | Complex-conjugate auto-pair |
| Triple freq panel | `freqz` over $[0,\pi]$ with 512 bins, unwrapped phase, numerical $-d\phi/d\omega$ | Per-filter selector |
| Elementary explorer | RBJ cookbook closed forms + $(\omega_0, Q, \text{dB})$ sliders | 4-panel layout |
| Min-phase reflect | Paired FIR zeros at $(r,\pm\omega)$ and $(1/r,\pm\omega)$ | Magnitude identical, phase different |
| Structure diagrams | Hand-drawn signal-flow: DF-I, DF-II, TDF-II, cascade | Pure Canvas |
| FIR/IIR designer | Window-method (sinc &times; window); bilinear-transform Butterworth | LP/HP with spectral reversal |
| Allpass cascade | $(a + z^{-1})/(1 + a z^{-1})$ chained $N$ times | $|H|$ verifies flat |
| EQ on drum loop | Three RBJ peaking biquads in cascade; `filterSig` for each | A/B audio via Web Audio |

---

## Key References

1. Smith, J. O. (2007). *Introduction to Digital Filters with Audio Applications.* W3K Publishing. [ccrma.stanford.edu/~jos/filters/](https://ccrma.stanford.edu/~jos/filters/)
2. Black, H. S. (1934). *Stabilized Feedback Amplifiers.* Electrical Engineering 53(1), 114-120.
3. Bode, H. W. (1945). *Network Analysis and Feedback Amplifier Design.* Van Nostrand.
4. Ragazzini, J. R. &amp; Zadeh, L. A. (1952). *The Analysis of Sampled-Data Systems.* Trans. AIEE 71(II), 225-234.
5. Cooley, J. W. &amp; Tukey, J. W. (1965). *An Algorithm for the Machine Calculation of Complex Fourier Series.* Math. Comp. 19(90), 297-301.
6. Kaiser, J. F. (1966). *Digital Filters.* In Kuo &amp; Kaiser (eds.), System Analysis by Digital Computer. Wiley.
7. Gold, B. &amp; Rader, C. M. (1969). *Digital Processing of Signals.* McGraw-Hill.
8. Parks, T. W. &amp; McClellan, J. H. (1972). *Chebyshev Approximation for Nonrecursive Digital Filters with Linear Phase.* IEEE Trans. Circuit Theory 19(2), 189-194.
9. Rabiner, L. R. &amp; Gold, B. (1975). *Theory and Application of Digital Signal Processing.* Prentice-Hall.
10. Oppenheim, A. V. &amp; Schafer, R. W. (1975). *Digital Signal Processing.* Prentice-Hall.
11. Schroeder, M. R. (1962). *Natural Sounding Artificial Reverberation.* J. Audio Eng. Soc. 10(3), 219-223.
12. Bristow-Johnson, R. (2005). *Cookbook Formulae for Audio EQ Biquad Filter Coefficients.* [W3C TR](https://www.w3.org/TR/audio-eq-cookbook/).

---

## Related Guides

* [Companion to *Mathematics of the DFT*](https://github.com/BrendanJamesLynskey/Companion_JOS_Mathematics_of_the_DFT) &mdash; basis expansions, DFT theorems.
* [Companion to *Spectral Audio Signal Processing*](https://github.com/BrendanJamesLynskey/Companion_JOS_Spectral_Audio_Signal_Processing) &mdash; STFT, spectral modelling.
* [Companion to *Physical Audio Signal Processing*](https://github.com/BrendanJamesLynskey/Companion_JOS_Physical_Audio_Signal_Processing) &mdash; digital waveguide synthesis.
* [Companion to *Audio Signal Processing in Faust*](https://github.com/BrendanJamesLynskey/Companion_JOS_Audio_Signal_Processing_in_Faust) &mdash; functional-DSP block diagrams.
* [STFT for Sound](https://github.com/BrendanJamesLynskey/STFT_for_Sound) &mdash; Gabor atoms, phase vocoder.
* [Laplace Guide](https://github.com/BrendanJamesLynskey/LaplaceGuide) &mdash; continuous-time transfer functions (the analogue side of the bilinear transform).
* [Minimum / Maximum Phase Filters](https://github.com/BrendanJamesLynskey/MinimumMaximumPhaseFilters) &mdash; zero-location story in depth.
* [Kramers-Kronig Relations](https://github.com/BrendanJamesLynskey/Kramers_Kronig_Relations) &mdash; Bode's magnitude-phase link in its physics form.
* [Sound Transforms &mdash; History and Practical Guide](https://github.com/BrendanJamesLynskey/Sound_Transforms_History) &mdash; family tree of audio transforms.

---

## License

MIT &mdash; use it however you like. Attribution appreciated but not required.
