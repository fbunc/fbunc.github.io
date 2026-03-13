# How `s_n(n)` is plotted in `StartHere.html`

This document focuses only on the `s_n` plotting path in `StartHere.html`.

## 1) Entry point used by the UI

When **Formula Set = "Factorization in 2D"**, `updatePlot()` routes to:

- `computeFactorizationCoordinatesJS(num_points, factorization_type, factorization_To, start_index, factorization_w_zero)`

So for `s_n`, the selected `factorization_type` is `"s_n"`.

## 2) Exact complex sequence used for `s_n`

`computeFactorizationCoordinatesJS(...)` loops over:

- `n = i + startN`, where `startN = max(1, start_index)`

For each `n`, it calls:

- `calculateFactorizationComplex(n, 's_n', To)`

Inside `calculateFactorizationComplex` for `sequenceType === 's_n'`:

1. Prime factors are obtained with multiplicity:
   - `factors = getPrimeFactors(n, false)`
2. It computes

$$
z_n = \sum_{p \in \text{prime factors of } n \text{ (with multiplicity)}} e^{i\,2\pi p/T_o}
$$

implemented as:

- `sumRe += cos(2π p / To)`
- `sumIm += sin(2π p / To)`

and returns `Complex(sumRe, sumIm)`.

So:

- `u_n = Re(z_n)`
- `v_n = Im(z_n)`

## 3) `w_n` behavior

In `computeFactorizationCoordinatesJS`:

- if `factorization_w_zero === true`, then `w_n = 0`
- else `w_n = log2(|z_n|)`

with `|z_n| = sqrt(u_n^2 + v_n^2)`.

## 4) What is actually drawn

The computed arrays are cached in `fullCoordDataCache` and passed to plot drawing.

For curve `A` (identity transform):

- point coordinates are directly `(u_n, v_n, w_n)`

Other enabled curves are symmetry transforms of this same base `(u_n, v_n, w_n)`.

## 5) Important indexing detail

Factorization mode uses `n` directly (not `n+1`) for prime/index display logic:

- `isFactorizationFormulaSet()`
- `getNumberValueFromPlotIndex(nVal)` returns `nVal` in factorization mode

This keeps `s_n(n)` indexing aligned with the plotted points.

## 6) Minimal pseudocode (faithful to file)

```text
startN = max(1, start_index)
for i in 0..num_points-1:
    n = i + startN
    factors = prime_factors_with_multiplicity(n)
    z = Σ exp(i*2π*p/To) over p in factors
    u[i] = Re(z)
    v[i] = Im(z)
    w[i] = 0 if force_w0 else log2(|z|)
```
