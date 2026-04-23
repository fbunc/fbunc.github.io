# Why mod 10? — the factorization compass in base 10

> An entry point into the factorization-in-2D work, starting from the most familiar question: *why does our number system use 10?* From there, the notebook walks into mod-6 prime spirals and the complex-vector $s_n$ construction.
>
> **Source:** `QUESTIONS ABOUT PRIMES/000_FACTORIZATION_MAN/000_why_mod_10.ipynb`.

# Why $T_o = 10$? — Primes Separated by Last Digit

This notebook demonstrates why $T_o = 10$ is the natural "decimal fingerprint" period for prime factorization in 2D.

**Three sequences (user notation):**

| Sequence | Definition | Description |
|----------|-----------|-------------|
| $s_n$ | $\sum_{k=1}^{M_n} \omega^{b_k}$ | All prime factors with multiplicity |
| $\dot{s}_n$ | $\sum_{k=1}^{m_n} \omega^{d_k}$ | Distinct prime factors only |
| $\ddot{s}_n$ | $\sum_{k=1}^{m_n} e_k \cdot \omega^{d_k}$ | Weighted by exponents |

where $\omega = e^{2\pi i / T_o}$, $b_k$ are prime factors with multiplicity, $d_k$ distinct primes, $e_k$ exponents, $M_n = \Omega(n)$, $m_n = \omega(n)$.

> **Note:** $s_n = \ddot{s}_n$ always holds (both count $\omega^p$ exactly $e_k$ times).

```python
import math, sys, os
from fractions import Fraction
from collections import Counter

# Import the symbolic engine
ENGINE_DIR = os.path.abspath(os.path.join('..', '0000_NOW_YOU_FACTORIZE'))
sys.path.insert(0, ENGINE_DIR)
from factorization_2d_symbolic import build_spf, factorize, is_prime, compute_numeric

EPS = 1e-10
To = 10
spf = build_spf(1000)

def expand_factors(n, _spf=None):
    # Flat list of prime factors with multiplicity (e.g. 12 -> [2,2,3])
    return [p for p, e in factorize(n, _spf or spf) for _ in range(e)]

def compute_sdot_sddot(n, _spf=None):
    # Return (sdot_re, sdot_im, sddot_re, sddot_im) for n
    fs = factorize(n, _spf or spf)
    sdot_re = sdot_im = sddot_re = sddot_im = 0.0
    for p, e in fs:
        theta = 2 * math.pi * p / To
        c, s = math.cos(theta), math.sin(theta)
        sdot_re += c;  sdot_im += s
        sddot_re += e * c;  sddot_im += e * s
    return sdot_re, sdot_im, sddot_re, sddot_im

print("Engine loaded.  SPF sieve built for n ≤ 1000.")
print(f"T_o = {To},  ω = e^(2πi/{To})")
```

```
Engine loaded.  SPF sieve built for n ≤ 1000.
T_o = 10,  ω = e^(2πi/10)
```

## 1. $\omega^p$ depends only on $p \bmod 10$

$$\omega = e^{2\pi i/T_o} \;\Longrightarrow\; \omega^p = e^{2\pi i\,p/T_o}$$

Since $e^{2\pi i(p+T_o)/T_o} = \omega^p \cdot e^{2\pi i} = \omega^p$, the angle depends only on $p \bmod T_o$.

At $T_o = 10$: **$\omega^p$ is determined by the last decimal digit of $p$.**

```python
# Verify: group all primes < 1000 by last digit
angle_by_digit = {}
for p in range(2, 1000):
    if not is_prime(p, spf):
        continue
    d = p % To
    theta_mod = (2 * math.pi * p / To) % (2 * math.pi)
    expected  = 2 * math.pi * d / To
    assert abs(theta_mod - expected) < EPS or abs(theta_mod - expected - 2*math.pi) < EPS, \
        f"p={p}: θ mod 2π = {theta_mod}, expected {expected}"
    angle_by_digit.setdefault(d, []).append(p)

print(f"Prime last digits appearing (p < 1000): {sorted(angle_by_digit.keys())}")
print(f"Non-trivial digits (p ≥ 5): {{1, 3, 7, 9}}")
print(f"Small primes 2, 3, 5 get their own digit.\n")
for d in sorted(angle_by_digit.keys()):
    first_few = angle_by_digit[d][:6]
    print(f"  digit {d}: {first_few}{'...' if len(angle_by_digit[d]) > 6 else ''}")
    assert all(p % 10 == d for p in angle_by_digit[d])
print(f"\n✓ All {sum(len(v) for v in angle_by_digit.values())} primes < 1000 verified: ω^p = ω^(p mod 10)")
```

```
Prime last digits appearing (p < 1000): [1, 2, 3, 5, 7, 9]
Non-trivial digits (p ≥ 5): {1, 3, 7, 9}
Small primes 2, 3, 5 get their own digit.

  digit 1: [11, 31, 41, 61, 71, 101]...
  digit 2: [2]
  digit 3: [3, 13, 23, 43, 53, 73]...
  digit 5: [5]
  digit 7: [7, 17, 37, 47, 67, 97]...
  digit 9: [19, 29, 59, 79, 89, 109]...

✓ All 168 primes < 1000 verified: ω^p = ω^(p mod 10)
```

## 2. The four prime-angle classes

For primes $p \ge 5$, only four last digits are possible: $\{1, 3, 7, 9\}$.

Each maps to a fixed angle on the unit circle. Including small primes $2, 3, 5$:

```python
# Angle data: (last_digit, degrees, fraction_of_2π, radians)
angle_data = [
    (1, Fraction(1, 10), 36),
    (2, Fraction(2, 10), 72),
    (3, Fraction(3, 10), 108),
    (5, Fraction(5, 10), 180),
    (7, Fraction(7, 10), 252),
    (9, Fraction(9, 10), 324),
]

print(f"{'digit':>5}  {'θ / 2π':>8}  {'degrees':>8}  {'radians':>12}  {'primes (≤ 29)'}")
print(f"{'─'*5}  {'─'*8}  {'─'*8}  {'─'*12}  {'─'*20}")
for d, frac, deg in angle_data:
    rad = float(frac) * 2 * math.pi
    # Verify
    expected_rad = 2 * math.pi * d / To
    assert abs(rad - expected_rad) < EPS
    assert abs(deg - float(frac) * 360) < EPS
    ps = [p for p in range(2, 30) if is_prime(p, spf) and p % 10 == d]
    frac_str = f"{frac.numerator}/{frac.denominator}"
    print(f"  {d:>3}    {frac_str:>6}  {deg:>7}°  {rad:>11.6f}  {ps}")

print("\n✓ All angles verified: θ = 2π × (digit / 10)")
print("\nKey: primes ≥ 5 use only 4 of these 6 rows (digits 1, 3, 7, 9)")
print("     primes 2, 3, 5 each occupy their own unique angle.")
```

```
digit    θ / 2π   degrees       radians  primes (≤ 29)
─────  ────────  ────────  ────────────  ────────────────────
    1      1/10       36°     0.628319  [11]
    2       1/5       72°     1.256637  [2]
    3      3/10      108°     1.884956  [3, 13, 23]
    5       1/2      180°     3.141593  [5]
    7      7/10      252°     4.398230  [7, 17]
    9      9/10      324°     5.654867  [19, 29]

✓ All angles verified: θ = 2π × (digit / 10)

Key: primes ≥ 5 use only 4 of these 6 rows (digits 1, 3, 7, 9)
     primes 2, 3, 5 each occupy their own unique angle.
```

## 3. Symmetries: conjugate pairs and $\omega^5 = -1$

The four prime angles (for $p \ge 5$) pair into conjugates:
- $\omega^1$ and $\omega^9$ are conjugates ($36° + 324° = 360°$)
- $\omega^3$ and $\omega^7$ are conjugates ($108° + 252° = 360°$)

Also: $\omega^5 = e^{i\pi} = -1$, so $\omega^{a+5} = -\omega^a$ (antipodal on the circle).

```python
import cmath

# Verify conjugate pairs
w = cmath.exp(2j * math.pi / To)
for a, b in [(1, 9), (3, 7)]:
    wa, wb = w**a, w**b
    assert abs(wa - wb.conjugate()) < EPS, f"ω^{a} ≠ conj(ω^{b})"
    print(f"  ω^{a} = {wa:.6f},  ω^{b} = {wb:.6f}  ← conjugate pair ✓")

# Verify ω^5 = -1
assert abs(w**5 - (-1)) < EPS
print(f"\n  ω^5 = {w**5:.6f} = -1  ✓")

# Verify antipodal: ω^{a+5} = -ω^a
for a in range(10):
    assert abs(w**((a + 5) % 10) + w**a) < EPS
print(f"  ω^{{a+5}} = -ω^a for all a ∈ {{0..9}}  ✓")

# Cancellation pairs
print("\n  Antipodal cancellation pairs (d, d+5 mod 10):")
pairs = [(d, (d + 5) % 10) for d in range(5)]
for a, b in pairs:
    res = w**a + w**b
    assert abs(res) < EPS
    viable = "✓ VIABLE" if {a, b} <= {1, 2, 3, 5, 7, 9} else "✗ non-prime digit"
    print(f"    ({a},{b}):  ω^{a} + ω^{b} = 0   {viable}")

print("\n  → Among prime last digits {1,2,3,5,7,9}, the ONLY viable")
print("    antipodal pair is (2, 7).")
```

```
  ω^1 = 0.809017+0.587785j,  ω^9 = 0.809017-0.587785j  ← conjugate pair ✓
  ω^3 = -0.309017+0.951057j,  ω^7 = -0.309017-0.951057j  ← conjugate pair ✓

  ω^5 = -1.000000+0.000000j = -1  ✓
  ω^{a+5} = -ω^a for all a ∈ {0..9}  ✓

  Antipodal cancellation pairs (d, d+5 mod 10):
    (0,5):  ω^0 + ω^5 = 0   ✗ non-prime digit
    (1,6):  ω^1 + ω^6 = 0   ✗ non-prime digit
    (2,7):  ω^2 + ω^7 = 0   ✓ VIABLE
    (3,8):  ω^3 + ω^8 = 0   ✗ non-prime digit
    (4,9):  ω^4 + ω^9 = 0   ✗ non-prime digit

  → Among prime last digits {1,2,3,5,7,9}, the ONLY viable
    antipodal pair is (2, 7).
```

## 4. How $s_n$ respects mod-10 structure

Since $s_n = \sum \omega^{b_k}$ and each $\omega^{b_k} = \omega^{b_k \bmod 10}$,
the value of $s_n$ depends only on the **multiset of last digits** of the prime factors of $n$.

If two numbers $n, m$ share the same last-digit multiset, then $s_n = s_m$.

```python
# Example 1:  n = 34 = 2 × 17  → digits [2, 7]
# s_34 = ω² + ω⁷ = ω² + ω^{2+5} = ω²(1 + ω⁵) = ω²(1-1) = 0
sn34 = compute_numeric(34, To, spf)
assert abs(sn34[0]) < EPS and abs(sn34[1]) < EPS
print(f"n = 34 = 2 × 17  → digits [2, 7]")
print(f"  s_34 = ω² + ω⁷ = 0  ✓  (antipodal pair)\n")

# Example 2:  n = 12 = 2² × 3  → digits [2, 2, 3]
sn12 = compute_numeric(12, To, spf)
print(f"n = 12 = 2² × 3  → digits [2, 2, 3]")
print(f"  s_12 = 2ω² + ω³ = ({sn12[0]:.6f}, {sn12[1]:.6f})  ≠ 0\n")

# Example 3:  n = 91 = 7 × 13  → digits [7, 3]
# n = 21 = 3 × 7   → digits [3, 7]  — same multiset!
sn91 = compute_numeric(91, To, spf)
sn21 = compute_numeric(21, To, spf)
assert abs(sn91[0] - sn21[0]) < EPS and abs(sn91[1] - sn21[1]) < EPS
print(f"n = 91 = 7 × 13  → digits [7, 3]")
print(f"n = 21 = 3 × 7   → digits [3, 7]  ← same multiset")
print(f"  s_91 = ({sn91[0]:.6f}, {sn91[1]:.6f})")
print(f"  s_21 = ({sn21[0]:.6f}, {sn21[1]:.6f})")
print(f"  s_91 = s_21  ✓  (same last-digit multiset)")
```

```
n = 34 = 2 × 17  → digits [2, 7]
  s_34 = ω² + ω⁷ = 0  ✓  (antipodal pair)

n = 12 = 2² × 3  → digits [2, 2, 3]
  s_12 = 2ω² + ω³ = (0.309017, 2.853170)  ≠ 0

n = 91 = 7 × 13  → digits [7, 3]
n = 21 = 3 × 7   → digits [3, 7]  ← same multiset
  s_91 = (-0.618034, 0.000000)
  s_21 = (-0.618034, 0.000000)
  s_91 = s_21  ✓  (same last-digit multiset)
```

## 5. Dirichlet's theorem: equal density per ray

By Dirichlet's theorem on primes in arithmetic progressions, for each residue class $a \in \{1, 3, 7, 9\}$, the density of primes $p \equiv a \pmod{10}$ is $\frac{1}{\varphi(10)} = \frac{1}{4}$.

This means the four angle-rays receive approximately **equal density** of prime points — creating the characteristic **4-fold symmetric cross pattern** at $T_o = 10$.

```python
# Count primes by last digit
counts = {1: 0, 3: 0, 7: 0, 9: 0}
for p in range(5, 1000):
    if is_prime(p, spf):
        d = p % 10
        if d in counts:
            counts[d] += 1
total = sum(counts.values())

print(f"Primes 5 ≤ p < 1000 by last digit:\n")
for d in [1, 3, 7, 9]:
    frac = counts[d] / total
    bar = '█' * int(frac * 80)
    print(f"  digit {d}: {counts[d]:>3} primes  ({frac:.1%})  {bar}")
    assert 0.20 < frac < 0.30, f"digit {d}: fraction {frac} out of range"

print(f"\n  Total: {total} primes")
print(f"✓ Verified approximate equidistribution (each ≈ 25%)")
```

```
Primes 5 ≤ p < 1000 by last digit:

  digit 1:  40 primes  (24.2%)  ███████████████████
  digit 3:  41 primes  (24.8%)  ███████████████████
  digit 7:  46 primes  (27.9%)  ██████████████████████
  digit 9:  38 primes  (23.0%)  ██████████████████

  Total: 165 primes
✓ Verified approximate equidistribution (each ≈ 25%)
```

## 6. Complete census of $s_n = 0$  ($n \le 1000$, $T_o = 10$)

Recall:
$$s_n = \sum_{k=1}^{M_n} \omega^{b_k}, \qquad \dot{s}_n = \sum_{k=1}^{m_n} \omega^{d_k}, \qquad \ddot{s}_n = \sum_{k=1}^{m_n} e_k \cdot \omega^{d_k}$$

$s_n = 0$ means the unit-vector sum cancels completely.

### The cancellation mechanism

Since $\omega^{a+5} = \omega^a \cdot \omega^5 = \omega^a \cdot (-1) = -\omega^a$,
any two factors whose last digits differ by 5 cancel pairwise:

$$\omega^a + \omega^{a+5} = 0 \qquad \text{(antipodal on the unit circle)}$$

Among prime last digits $\{1, 2, 3, 5, 7, 9\}$, the only viable antipodal pair is $\{2, 7\}$.

```python
# Find all n ≤ 1000 with s_n = 0
N_SCAN = 1000
zeros = []
for n in range(2, N_SCAN + 1):
    sn_re, sn_im, _, _ = compute_numeric(n, To, spf)
    if abs(sn_re) < EPS and abs(sn_im) < EPS:
        zeros.append(n)

print(f"Found {len(zeros)} values of n ≤ {N_SCAN} with s_n = 0.\n")

# Classify zeros
type_pairwise = []
type_complex  = []
for n in zeros:
    fs = expand_factors(n)
    digits = [p % 10 for p in fs]
    dcnt = Counter(digits)
    if set(dcnt.keys()) <= {2, 7} and dcnt.get(2, 0) == dcnt.get(7, 0):
        type_pairwise.append(n)
    else:
        type_complex.append(n)

print(f"  Type A (pairwise 2↔7): {len(type_pairwise)} cases")
print(f"  Type B (complex):      {len(type_complex)} cases")
```

```
Found 26 values of n ≤ 1000 with s_n = 0.

  Type A (pairwise 2↔7): 26 cases
  Type B (complex):      0 cases
```

```python
# Type A: pairwise (2↔7) cancellation — full table
print(f"{'─'*64}")
print(f"TYPE A: Pairwise (2↔7) cancellation  ({len(type_pairwise)} cases)")
print(f"{'─'*64}")
print(f"n = 2^a × p₁ × p₂ × ... where each pᵢ ends in 7,")
print(f"and #(factors ending in 2) = #(factors ending in 7).\n")

print(f'{"n":>6}  {"factorization":<22} {"digits":>16}  {"s_n":>6}  {"ṡ_n":>6}  {"s̈_n":>6}  mechanism')
print(f'{"─"*6}  {"─"*22} {"─"*16}  {"─"*6}  {"─"*6}  {"─"*6}  {"─"*20}')

for n in type_pairwise:
    fs_pe = factorize(n, spf)
    parts = []
    for p, e in fs_pe:
        parts.append(f"{p}^{e}" if e > 1 else str(p))
    factors_str = " × ".join(parts)

    fs = expand_factors(n)
    digits = [p % 10 for p in fs]
    dstr = ",".join(str(d) for d in digits)

    sn_re, sn_im, _, _ = compute_numeric(n, To, spf)
    sdot_re, sdot_im, sddot_re, sddot_im = compute_sdot_sddot(n)

    sn_zero   = abs(sn_re) < EPS and abs(sn_im) < EPS
    sdot_zero = abs(sdot_re) < EPS and abs(sdot_im) < EPS
    sddot_zero = abs(sddot_re) < EPS and abs(sddot_im) < EPS

    assert sn_zero, f"s_{n} ≠ 0!"
    assert sddot_zero, f"s̈_{n} should equal s_n = 0"

    sn_sym   = "= 0" if sn_zero else "≠ 0"
    sdot_sym = "= 0" if sdot_zero else "≠ 0"
    sddot_sym = "= 0" if sddot_zero else "≠ 0"

    pairs = min(Counter(digits).get(2, 0), Counter(digits).get(7, 0))
    mech = f"{pairs}×(ω²+ω⁷)=0"

    print(f'{n:>6}  = {factors_str:<20} [{dstr:>14}]  {sn_sym:>6}  {sdot_sym:>6}  {sddot_sym:>6}  {mech}')

print(f"\n✓ All {len(type_pairwise)} Type A zeros: s_n = s̈_n = 0 asserted")
```

```
────────────────────────────────────────────────────────────────
TYPE A: Pairwise (2↔7) cancellation  (26 cases)
────────────────────────────────────────────────────────────────
n = 2^a × p₁ × p₂ × ... where each pᵢ ends in 7,
and #(factors ending in 2) = #(factors ending in 7).

     n  factorization                    digits     s_n     ṡ_n    s̈_n  mechanism
──────  ────────────────────── ────────────────  ──────  ──────  ──────  ────────────────────
    14  = 2 × 7                [           2,7]     = 0     = 0     = 0  1×(ω²+ω⁷)=0
    34  = 2 × 17               [           2,7]     = 0     = 0     = 0  1×(ω²+ω⁷)=0
    74  = 2 × 37               [           2,7]     = 0     = 0     = 0  1×(ω²+ω⁷)=0
    94  = 2 × 47               [           2,7]     = 0     = 0     = 0  1×(ω²+ω⁷)=0
   134  = 2 × 67               [           2,7]     = 0     = 0     = 0  1×(ω²+ω⁷)=0
   194  = 2 × 97               [           2,7]     = 0     = 0     = 0  1×(ω²+ω⁷)=0
   196  = 2^2 × 7^2            [       2,2,7,7]     = 0     = 0     = 0  2×(ω²+ω⁷)=0
   214  = 2 × 107              [           2,7]     = 0     = 0     = 0  1×(ω²+ω⁷)=0
   254  = 2 × 127              [           2,7]     = 0     = 0     = 0  1×(ω²+ω⁷)=0
   274  = 2 × 137              [           2,7]     = 0     = 0     = 0  1×(ω²+ω⁷)=0
   314  = 2 × 157              [           2,7]     = 0     = 0     = 0  1×(ω²+ω⁷)=0
   334  = 2 × 167              [           2,7]     = 0     = 0     = 0  1×(ω²+ω⁷)=0
   394  = 2 × 197              [           2,7]     = 0     = 0     = 0  1×(ω²+ω⁷)=0
   454  = 2 × 227              [           2,7]     = 0     = 0     = 0  1×(ω²+ω⁷)=0
   476  = 2^2 × 7 × 17         [       2,2,7,7]     = 0     ≠ 0     = 0  2×(ω²+ω⁷)=0
   514  = 2 × 257              [           2,7]     = 0     = 0     = 0  1×(ω²+ω⁷)=0
   554  = 2 × 277              [           2,7]     = 0     = 0     = 0  1×(ω²+ω⁷)=0
   614  = 2 × 307              [           2,7]     = 0     = 0     = 0  1×(ω²+ω⁷)=0
   634  = 2 × 317              [           2,7]     = 0     = 0     = 0  1×(ω²+ω⁷)=0
   674  = 2 × 337              [           2,7]     = 0     = 0     = 0  1×(ω²+ω⁷)=0
   694  = 2 × 347              [           2,7]     = 0     = 0     = 0  1×(ω²+ω⁷)=0
   734  = 2 × 367              [           2,7]     = 0     = 0     = 0  1×(ω²+ω⁷)=0
   794  = 2 × 397              [           2,7]     = 0     = 0     = 0  1×(ω²+ω⁷)=0
   914  = 2 × 457              [           2,7]     = 0     = 0     = 0  1×(ω²+ω⁷)=0
   934  = 2 × 467              [           2,7]     = 0     = 0     = 0  1×(ω²+ω⁷)=0
   974  = 2 × 487              [           2,7]     = 0     = 0     = 0  1×(ω²+ω⁷)=0

✓ All 26 Type A zeros: s_n = s̈_n = 0 asserted
```

```python
# ─── DETAILED BREAKDOWNS ───

# n = 34
print("▸ n = 34 = 2 × 17")
print("  Factorization: b_k = [2, 17],  d_k = [2, 17],  e_k = [1, 1]")
print("  M_n = Ω(34) = 2,  m_n = ω(34) = 2")
print("  Last digits: [2, 7]")
print("  s_34  = ω² + ω^{17} = ω² + ω⁷")
print("        = ω²(1 + ω⁵) = ω²(1 + (-1)) = ω² · 0 = 0  ✓")
print("  ṡ_34  = ω² + ω^{17} = ω² + ω⁷ = 0   (same, since all e_k = 1)")
print("  s̈_34  = 1·ω² + 1·ω^{17} = ω² + ω⁷ = 0   ✓")
sn34 = compute_numeric(34, To, spf)
sd34 = compute_sdot_sddot(34)
assert abs(sn34[0]) < EPS and abs(sn34[1]) < EPS
assert abs(sd34[0]) < EPS and abs(sd34[1]) < EPS
assert abs(sd34[2]) < EPS and abs(sd34[3]) < EPS
print("  → All three sequences vanish: s_34 = ṡ_34 = s̈_34 = 0  ✓\n")

# n = 196
print("▸ n = 196 = 2² × 7²")
print("  Factorization: b_k = [2,2,7,7],  d_k = [2,7],  e_k = [2,2]")
print("  M_n = 4,  m_n = 2")
print("  Last digits: [2,2,7,7]")
print("  s_196  = 2ω² + 2ω⁷ = 2(ω² + ω⁷) = 2·0 = 0     ✓")
print("  ṡ_196  = ω² + ω⁷ = 0                              ✓")
print("  s̈_196  = 2ω² + 2ω⁷ = 2(ω² + ω⁷) = 0             ✓")
sn196 = compute_numeric(196, To, spf)
sd196 = compute_sdot_sddot(196)
assert abs(sn196[0]) < EPS and abs(sn196[1]) < EPS
assert abs(sd196[0]) < EPS and abs(sd196[1]) < EPS
assert abs(sd196[2]) < EPS and abs(sd196[3]) < EPS
print("  → All three vanish: s_196 = ṡ_196 = s̈_196 = 0  ✓\n")

# n = 476
print("▸ n = 476 = 2² × 7 × 17")
print("  Factorization: b_k = [2,2,7,17],  d_k = [2,7,17],  e_k = [2,1,1]")
print("  M_n = 4,  m_n = 3")
print("  Last digits: [2,2,7,7]  (since 17 mod 10 = 7)")
print("  s_476  = 2ω² + ω⁷ + ω⁷ = 2ω² + 2ω⁷ = 2(ω²+ω⁷) = 0  ✓")
print("  s̈_476  = 2ω² + 1·ω⁷ + 1·ω^{17} = 2ω² + 2ω⁷ = 0      ✓")
print("  BUT ṡ_476  = ω² + ω⁷ + ω^{17} = ω² + ω⁷ + ω⁷")
print("            = ω² + 2ω⁷ ≠ 0   ← distinct primes differ!")
sn476 = compute_numeric(476, To, spf)
sd476 = compute_sdot_sddot(476)
assert abs(sn476[0]) < EPS and abs(sn476[1]) < EPS
assert abs(sd476[2]) < EPS and abs(sd476[3]) < EPS
sdot_476_zero = abs(sd476[0]) < EPS and abs(sd476[1]) < EPS
assert not sdot_476_zero, "ṡ_476 should NOT be 0"
print(f"  ṡ_476 = ({sd476[0]:.6f}, {sd476[1]:.6f})")
print("  → s_476 = s̈_476 = 0, but ṡ_476 ≠ 0")
print("    Reason: d_k = [2, 7, 17] has 3 distinct primes,")
print("    but digits [2, 7, 7] can't pair evenly as (2↔7).  ✓")
```

```
▸ n = 34 = 2 × 17
  Factorization: b_k = [2, 17],  d_k = [2, 17],  e_k = [1, 1]
  M_n = Ω(34) = 2,  m_n = ω(34) = 2
  Last digits: [2, 7]
  s_34  = ω² + ω^{17} = ω² + ω⁷
        = ω²(1 + ω⁵) = ω²(1 + (-1)) = ω² · 0 = 0  ✓
  ṡ_34  = ω² + ω^{17} = ω² + ω⁷ = 0   (same, since all e_k = 1)
  s̈_34  = 1·ω² + 1·ω^{17} = ω² + ω⁷ = 0   ✓
  → All three sequences vanish: s_34 = ṡ_34 = s̈_34 = 0  ✓

▸ n = 196 = 2² × 7²
  Factorization: b_k = [2,2,7,7],  d_k = [2,7],  e_k = [2,2]
  M_n = 4,  m_n = 2
  Last digits: [2,2,7,7]
  s_196  = 2ω² + 2ω⁷ = 2(ω² + ω⁷) = 2·0 = 0     ✓
  ṡ_196  = ω² + ω⁷ = 0                              ✓
  s̈_196  = 2ω² + 2ω⁷ = 2(ω² + ω⁷) = 0             ✓
  → All three vanish: s_196 = ṡ_196 = s̈_196 = 0  ✓

▸ n = 476 = 2² × 7 × 17
  Factorization: b_k = [2,2,7,17],  d_k = [2,7,17],  e_k = [2,1,1]
  M_n = 4,  m_n = 3
  Last digits: [2,2,7,7]  (since 17 mod 10 = 7)
  s_476  = 2ω² + ω⁷ + ω⁷ = 2ω² + 2ω⁷ = 2(ω²+ω⁷) = 0  ✓
  s̈_476  = 2ω² + 1·ω⁷ + 1·ω^{17} = 2ω² + 2ω⁷ = 0      ✓
  BUT ṡ_476  = ω² + ω⁷ + ω^{17} = ω² + ω⁷ + ω⁷
            = ω² + 2ω⁷ ≠ 0   ← distinct primes differ!
  ṡ_476 = (-0.309017, -0.951057)
  → s_476 = s̈_476 = 0, but ṡ_476 ≠ 0
    Reason: d_k = [2, 7, 17] has 3 distinct primes,
    but digits [2, 7, 7] can't pair evenly as (2↔7).  ✓
```

```python
# ─── WHEN DOES ṡ_n ALSO VANISH? ───
print("Since ṡ_n uses only distinct primes (each counted once),")
print("ṡ_n = 0 requires the distinct-prime last-digit multiset")
print("to cancel — which is stricter than s_n = 0.\n")

sdot_also_zero = []
sdot_nonzero = []
for n in zeros:
    sdot_re, sdot_im, _, _ = compute_sdot_sddot(n)
    if abs(sdot_re) < EPS and abs(sdot_im) < EPS:
        sdot_also_zero.append(n)
    else:
        sdot_nonzero.append(n)

print(f"Of the {len(zeros)} zeros (s_n = 0):")
print(f"  ṡ_n = 0 as well:  {len(sdot_also_zero)} cases  (all three sequences vanish)")
print(f"  ṡ_n ≠ 0:          {len(sdot_nonzero)} cases  (only s_n = s̈_n = 0)\n")

if sdot_nonzero:
    print("Cases where s_n = 0 but ṡ_n ≠ 0:")
    for n in sdot_nonzero:
        fs_pe = factorize(n, spf)
        parts = [f"{p}^{e}" if e > 1 else str(p) for p, e in fs_pe]
        fstr = " × ".join(parts)
        sdot_re, sdot_im, _, _ = compute_sdot_sddot(n)
        distinct_digits = sorted([p % 10 for p, e in fs_pe])
        print(f"  n = {n:>5} = {fstr:<20}  d_k digits={distinct_digits}  ṡ=({sdot_re:.4f},{sdot_im:.4f})")
    print()

print("Key insight: ṡ_n ≠ 0 happens when n has multiple distinct primes")
print("sharing the same last digit (e.g. 7 and 17 both → digit 7),")
print("breaking the even pairing needed for cancellation.\n")

# Verify semiprimes
semiprime_27 = []
for n in range(2, N_SCAN + 1):
    fs = expand_factors(n)
    if len(fs) == 2:
        d = sorted([f % 10 for f in fs])
        if d == [2, 7]:
            semiprime_27.append(n)
            sn_re, sn_im, _, _ = compute_numeric(n, To, spf)
            assert abs(sn_re) < EPS and abs(sn_im) < EPS
assert semiprime_27 == [n for n in type_pairwise if len(expand_factors(n)) == 2]
print(f"✓ All {len(semiprime_27)} semiprimes 2 × (prime ending in 7) give s_n = 0")
print(f"  {semiprime_27[:10]}{'...' if len(semiprime_27) > 10 else ''}")
```

```
Since ṡ_n uses only distinct primes (each counted once),
ṡ_n = 0 requires the distinct-prime last-digit multiset
to cancel — which is stricter than s_n = 0.

Of the 26 zeros (s_n = 0):
  ṡ_n = 0 as well:  25 cases  (all three sequences vanish)
  ṡ_n ≠ 0:          1 cases  (only s_n = s̈_n = 0)

Cases where s_n = 0 but ṡ_n ≠ 0:
  n =   476 = 2^2 × 7 × 17          d_k digits=[2, 7, 7]  ṡ=(-0.3090,-0.9511)

Key insight: ṡ_n ≠ 0 happens when n has multiple distinct primes
sharing the same last digit (e.g. 7 and 17 both → digit 7),
breaking the even pairing needed for cancellation.

✓ All 24 semiprimes 2 × (prime ending in 7) give s_n = 0
  [14, 34, 74, 94, 134, 194, 214, 254, 274, 314]...
```

## 6b. Type B: 5th-root-of-unity cancellations

Beyond pairwise $(2\leftrightarrow7)$, another way to get $s_n = 0$ uses the identity:

$$\sum_{k=0}^{4} (\omega^2)^k = 1 + \omega^2 + \omega^4 + \omega^6 + \omega^8 = 0$$

(sum of all 5th roots of unity, since $\omega^2$ is a primitive 5th root).

Multiplying by $\omega$:
$$\omega + \omega^3 + \omega^5 + \omega^7 + \omega^9 = 0$$

The odd residues mod 10 form a rotated set of 5th roots!

```python
# Verify 5th-root identities
even_sum = sum(complex(math.cos(2*math.pi*k/10), math.sin(2*math.pi*k/10))
               for k in [0, 2, 4, 6, 8])
odd_sum  = sum(complex(math.cos(2*math.pi*k/10), math.sin(2*math.pi*k/10))
               for k in [1, 3, 5, 7, 9])
assert abs(even_sum) < EPS, "Even 5th-root sum should be 0"
assert abs(odd_sum)  < EPS, "Odd 5th-root sum should be 0"
print("✓ Σ ω^{0,2,4,6,8} = 0  (5th roots of unity)")
print("✓ Σ ω^{1,3,5,7,9} = 0  (rotated 5th roots)\n")

# Smallest Type B: n = 3 × 5 × 7 × 11 × 19 = 22155
typeB_n = 3 * 5 * 7 * 11 * 19
print(f"▸ Smallest Type B zero: n = {typeB_n} = 3 × 5 × 7 × 11 × 19")
print(f"  b_k = [3, 5, 7, 11, 19],  M_n = 5")
print(f"  Last digits: [3, 5, 7, 1, 9] → sorted: {{1, 3, 5, 7, 9}}")
print(f"  s_{{22155}} = ω¹ + ω³ + ω⁵ + ω⁷ + ω⁹")
print(f"           = ω · Σ_{{k=0}}^{{4}} (ω²)^k = ω · 0 = 0  ✓")

spf_big = build_spf(max(N_SCAN, typeB_n) + 10)
sn_re, sn_im, _, _ = compute_numeric(typeB_n, To, spf_big)
assert abs(sn_re) < EPS and abs(sn_im) < EPS
sdot_re, sdot_im, sddot_re, sddot_im = compute_sdot_sddot(typeB_n, spf_big)
assert abs(sdot_re) < EPS and abs(sdot_im) < EPS
assert abs(sddot_re) < EPS and abs(sddot_im) < EPS
print(f"  Numeric: s = ({sn_re:.1e}, {sn_im:.1e})")
print(f"  ṡ = s̈ = 0  (all e_k = 1, so all three coincide)  ✓\n")

# More Type B examples
typeB_examples = [
    (3 * 5 * 7 * 11 * 29,  "3 × 5 × 7 × 11 × 29"),
    (3 * 5 * 7 * 31 * 19,  "3 × 5 × 7 × 19 × 31"),
    (3 * 5 * 17 * 11 * 19, "3 × 5 × 11 × 17 × 19"),
    (5 * 13 * 7 * 11 * 19, "5 × 7 × 11 × 13 × 19"),
]

max_n_needed = max(n_ex for n_ex, _ in typeB_examples)
if max_n_needed + 10 > len(spf_big):
    spf_big = build_spf(max_n_needed + 10)

print("More Type B examples (all with digit set {1,3,5,7,9}):\n")
for n_ex, fstr in typeB_examples:
    fs_ex = [p for p, e in factorize(n_ex, spf_big) for _ in range(e)]
    digs = sorted([p % 10 for p in fs_ex])
    sn_re, sn_im, _, _ = compute_numeric(n_ex, To, spf_big)
    assert abs(sn_re) < EPS and abs(sn_im) < EPS
    print(f"  n = {n_ex:>6} = {fstr:<24}  digits={digs}  s_n=0  ✓")

print(f"\nWhy Type B zeros require large n:")
print(f"  Need M_n ≥ 5 with digits covering all of {{1,3,5,7,9}}.")
print(f"  Smallest = 3 × 5 × 7 × 11 × 19 = {typeB_n} (far beyond n ≤ {N_SCAN})")
print(f"  In contrast, smallest Type A zero is just 2 × 7 = 14.")
```

```
✓ Σ ω^{0,2,4,6,8} = 0  (5th roots of unity)
✓ Σ ω^{1,3,5,7,9} = 0  (rotated 5th roots)

▸ Smallest Type B zero: n = 21945 = 3 × 5 × 7 × 11 × 19
  b_k = [3, 5, 7, 11, 19],  M_n = 5
  Last digits: [3, 5, 7, 1, 9] → sorted: {1, 3, 5, 7, 9}
  s_{22155} = ω¹ + ω³ + ω⁵ + ω⁷ + ω⁹
           = ω · Σ_{k=0}^{4} (ω²)^k = ω · 0 = 0  ✓
  Numeric: s = (3.3e-16, -1.1e-15)
  ṡ = s̈ = 0  (all e_k = 1, so all three coincide)  ✓

More Type B examples (all with digit set {1,3,5,7,9}):

  n =  33495 = 3 × 5 × 7 × 11 × 29       digits=[1, 3, 5, 7, 9]  s_n=0  ✓
  n =  61845 = 3 × 5 × 7 × 19 × 31       digits=[1, 3, 5, 7, 9]  s_n=0  ✓
  n =  53295 = 3 × 5 × 11 × 17 × 19      digits=[1, 3, 5, 7, 9]  s_n=0  ✓
  n =  95095 = 5 × 7 × 11 × 13 × 19      digits=[1, 3, 5, 7, 9]  s_n=0  ✓

Why Type B zeros require large n:
  Need M_n ≥ 5 with digits covering all of {1,3,5,7,9}.
  Smallest = 3 × 5 × 7 × 11 × 19 = 21945 (far beyond n ≤ 1000)
  In contrast, smallest Type A zero is just 2 × 7 = 14.
```

### Complete list of zero-sum digit mechanisms

At $T_o = 10$, $s_n = 0$ if and only if the last-digit multiset of $b_k$ decomposes into pieces that each sum to zero.

The **atomic** zero-sum sets are:

| Type | Digit set | Terms | Viable for primes? |
|------|-----------|-------|--------------------|
| **Pairwise** | $\{d,\, d{+}5 \bmod 10\}$ | 2 | Only $\{2, 7\}$ |
| **5th roots** | $\{1, 3, 5, 7, 9\}$ | 5 | Yes — odd residues |

> The even set $\{0,2,4,6,8\}$ is impossible since digits $0,4,6,8$ never appear as prime last digits.

**Combined** examples decompose into unions: e.g. $\{2,7,1,3,5,7,9\} = \{2,7\} \cup \{1,3,5,7,9\}$.

```python
# Combined Type A + Type B example
n_combined = 14 * typeB_n
if n_combined + 10 > len(spf_big):
    spf_big = build_spf(n_combined + 10)
sn_re, sn_im, _, _ = compute_numeric(n_combined, To, spf_big)
assert abs(sn_re) < EPS and abs(sn_im) < EPS
fs_comb = [p for p, e in factorize(n_combined, spf_big) for _ in range(e)]
digs_comb = sorted([p % 10 for p in fs_comb])
print(f"Combined example: n = 14 × {typeB_n} = {n_combined}")
print(f"  digits = {digs_comb}")
print(f"  = {{2,7}} ∪ {{1,3,5,7,9}} → each subset sums to 0  ✓\n")

# Summary statistics
print(f"{'═'*64}")
print(f" SUMMARY OF s_n = 0 CASES")
print(f"{'═'*64}")
print(f"\n  Scan range n ≤ {N_SCAN}:  {len(zeros)} zeros found")
print(f"    Type A (pairwise 2↔7):         {len(type_pairwise)}")
print(f"    Type B (5th-root, n≥{typeB_n}):  0 in range (smallest = {typeB_n})")
print(f"    All three s=ṡ=s̈=0:            {len(sdot_also_zero)}")
print(f"    Only s=s̈=0, ṡ≠0:              {len(sdot_nonzero)}")
print(f"\n  ✓ All {len(zeros) + len(typeB_examples) + 1} zero cases + all angle identities asserted.")
```

```
Combined example: n = 14 × 21945 = 307230
  digits = [1, 2, 3, 5, 7, 7, 9]
  = {2,7} ∪ {1,3,5,7,9} → each subset sums to 0  ✓

════════════════════════════════════════════════════════════════
 SUMMARY OF s_n = 0 CASES
════════════════════════════════════════════════════════════════

  Scan range n ≤ 1000:  26 zeros found
    Type A (pairwise 2↔7):         26
    Type B (5th-root, n≥21945):  0 in range (smallest = 21945)
    All three s=ṡ=s̈=0:            25
    Only s=s̈=0, ṡ≠0:              1

  ✓ All 31 zero cases + all angle identities asserted.
```

## 7. The cross pattern

At $T_o = 10$, the conjugate symmetries ($1\leftrightarrow 9$, $3\leftrightarrow 7$) guarantee that the plot of $s_n$ is **symmetric about the real axis**.

The 4 prime angles form two conjugate pairs:
- $\omega^1, \omega^9$ at $\pm 36°$ from the positive real axis
- $\omega^3, \omega^7$ at $\pm 108°$ from the positive real axis (= $\pm 72°$ from negative real)

Composite $s_n$ values are vector sums of these, so they cluster along the directions bisecting and spanning these 4 rays — forming a **cross pattern** centered at the origin.

The cross arms align with the **real axis** (from the $1\leftrightarrow 9$ pair) and the **imaginary axis** (from constructive/destructive interference between the two pairs).

## Summary

$T_o = 10$ is the natural "decimal fingerprint" period because:

1. $\omega^p$ depends only on the **last digit** of $p$
2. Primes $\ge 5$ have exactly 4 possible last digits: $\{1, 3, 7, 9\}$
3. These map to 4 angles at $36°, 108°, 252°, 324°$ (i.e. $2\pi k/10$ for $k \in \{1,3,7,9\}$)
4. The angles come in **conjugate pairs** → real-axis symmetry
5. Dirichlet's theorem → **equal density** per ray → 4-fold cross
6. $s_n = 0$ arises from two mechanisms:
   - **Pairwise:** prime last-digits $\{2, 7\}$ cancel ($\omega^2 + \omega^7 = 0$)
   - **Multi-vector:** 5 terms forming $\{1,3,5,7,9\}$ → 5th-root sum $= 0$
7. When $s_n = 0$, $\ddot{s}_n = 0$ always holds (same sum). But $\dot{s}_n = 0$ only when distinct primes also pair/cancel.

> **All zero cases + all angle identities asserted.** ✓

---

# $T_o = 6$ Analysis — Primes Separated by Residue mod 6

We now run the **same analysis** as for $T_o = 10$, but with $T_o = 6$ where $\omega = e^{2\pi i/6}$ is a primitive 6th root of unity.

At $T_o = 6$: $\omega^k = \cos(2\pi k/6) + i\sin(2\pi k/6)$, so every $\operatorname{Re}(s_n)$ is of the form $\tfrac{a}{2}$ and every $\operatorname{Im}(s_n)$ is of the form $\tfrac{b\sqrt{3}}{2}$ for integers $a, b$.

### Exact values for $n = 1, \ldots, 30$

| $n$ | $\operatorname{Re}(s_n)$ | $\operatorname{Im}(s_n)$ | $\operatorname{Re}(\dot{s}_n)$ | $\operatorname{Im}(\dot{s}_n)$ |
|----:|:--------------------------|:--------------------------|:-------------------------------|:-------------------------------|
| 1 | $0$ | $0$ | $0$ | $0$ |
| 2 | $\frac{-1}{2}$ | $\frac{\sqrt{3}}{2}$ | $\frac{-1}{2}$ | $\frac{\sqrt{3}}{2}$ |
| 3 | $-1$ | $0$ | $-1$ | $0$ |
| 4 | $-1$ | $\sqrt{3}$ | $\frac{-1}{2}$ | $\frac{\sqrt{3}}{2}$ |
| 5 | $\frac{1}{2}$ | $-\frac{\sqrt{3}}{2}$ | $\frac{1}{2}$ | $-\frac{\sqrt{3}}{2}$ |
| 6 | $\frac{-3}{2}$ | $\frac{\sqrt{3}}{2}$ | $\frac{-3}{2}$ | $\frac{\sqrt{3}}{2}$ |
| 7 | $\frac{1}{2}$ | $\frac{\sqrt{3}}{2}$ | $\frac{1}{2}$ | $\frac{\sqrt{3}}{2}$ |
| 8 | $\frac{-3}{2}$ | $\frac{3\sqrt{3}}{2}$ | $\frac{-1}{2}$ | $\frac{\sqrt{3}}{2}$ |
| 9 | $-2$ | $0$ | $-1$ | $0$ |
| 10 | $0$ | $0$ | $0$ | $0$ |
| 11 | $\frac{1}{2}$ | $-\frac{\sqrt{3}}{2}$ | $\frac{1}{2}$ | $-\frac{\sqrt{3}}{2}$ |
| 12 | $-2$ | $\sqrt{3}$ | $\frac{-3}{2}$ | $\frac{\sqrt{3}}{2}$ |
| 13 | $\frac{1}{2}$ | $\frac{\sqrt{3}}{2}$ | $\frac{1}{2}$ | $\frac{\sqrt{3}}{2}$ |
| 14 | $0$ | $\sqrt{3}$ | $0$ | $\sqrt{3}$ |
| 15 | $\frac{-1}{2}$ | $-\frac{\sqrt{3}}{2}$ | $\frac{-1}{2}$ | $-\frac{\sqrt{3}}{2}$ |
| 16 | $-2$ | $2\sqrt{3}$ | $\frac{-1}{2}$ | $\frac{\sqrt{3}}{2}$ |
| 17 | $\frac{1}{2}$ | $-\frac{\sqrt{3}}{2}$ | $\frac{1}{2}$ | $-\frac{\sqrt{3}}{2}$ |
| 18 | $\frac{-5}{2}$ | $\frac{\sqrt{3}}{2}$ | $\frac{-3}{2}$ | $\frac{\sqrt{3}}{2}$ |
| 19 | $\frac{1}{2}$ | $\frac{\sqrt{3}}{2}$ | $\frac{1}{2}$ | $\frac{\sqrt{3}}{2}$ |
| 20 | $\frac{-1}{2}$ | $\frac{\sqrt{3}}{2}$ | $0$ | $0$ |
| 21 | $\frac{-1}{2}$ | $\frac{\sqrt{3}}{2}$ | $\frac{-1}{2}$ | $\frac{\sqrt{3}}{2}$ |
| 22 | $0$ | $0$ | $0$ | $0$ |
| 23 | $\frac{1}{2}$ | $-\frac{\sqrt{3}}{2}$ | $\frac{1}{2}$ | $-\frac{\sqrt{3}}{2}$ |
| 24 | $\frac{-5}{2}$ | $\frac{3\sqrt{3}}{2}$ | $\frac{-3}{2}$ | $\frac{\sqrt{3}}{2}$ |
| 25 | $1$ | $-\sqrt{3}$ | $\frac{1}{2}$ | $-\frac{\sqrt{3}}{2}$ |
| 26 | $0$ | $\sqrt{3}$ | $0$ | $\sqrt{3}$ |
| 27 | $-3$ | $0$ | $-1$ | $0$ |
| 28 | $\frac{-1}{2}$ | $\frac{3\sqrt{3}}{2}$ | $0$ | $\sqrt{3}$ |
| 29 | $\frac{1}{2}$ | $-\frac{\sqrt{3}}{2}$ | $\frac{1}{2}$ | $-\frac{\sqrt{3}}{2}$ |
| 30 | $-1$ | $0$ | $-1$ | $0$ |

```python
# ═══════════════════════════════════════════════════════════════
#  T_o = 6 — Table Verification & Lattice Assertion  (n ≤ 1000)
# ═══════════════════════════════════════════════════════════════
To6 = 6
sqrt3 = math.sqrt(3)

# Expected values for n=1..30 encoded as (2·Re(s_n), 2·Im(s_n)/√3, 2·Re(ṡ_n), 2·Im(ṡ_n)/√3)
# i.e. Re(s_n) = a/2, Im(s_n) = b·√3/2
expected_6 = {
    1:  ( 0,  0,  0,  0),   2:  (-1,  1, -1,  1),
    3:  (-2,  0, -2,  0),   4:  (-2,  2, -1,  1),
    5:  ( 1, -1,  1, -1),   6:  (-3,  1, -3,  1),
    7:  ( 1,  1,  1,  1),   8:  (-3,  3, -1,  1),
    9:  (-4,  0, -2,  0),  10:  ( 0,  0,  0,  0),
   11:  ( 1, -1,  1, -1),  12:  (-4,  2, -3,  1),
   13:  ( 1,  1,  1,  1),  14:  ( 0,  2,  0,  2),
   15:  (-1, -1, -1, -1),  16:  (-4,  4, -1,  1),
   17:  ( 1, -1,  1, -1),  18:  (-5,  1, -3,  1),
   19:  ( 1,  1,  1,  1),  20:  (-1,  1,  0,  0),
   21:  (-1,  1, -1,  1),  22:  ( 0,  0,  0,  0),
   23:  ( 1, -1,  1, -1),  24:  (-5,  3, -3,  1),
   25:  ( 2, -2,  1, -1),  26:  ( 0,  2,  0,  2),
   27:  (-6,  0, -2,  0),  28:  (-1,  3,  0,  2),
   29:  ( 1, -1,  1, -1),  30:  (-2,  0, -2,  0),
}

def compute_sdot6(n):
    # Return (ṡ_re, ṡ_im) for n at T_o=6
    fs = factorize(n, spf)
    sr = si = 0.0
    for p, e in fs:
        theta = 2 * math.pi * p / To6
        sr += math.cos(theta);  si += math.sin(theta)
    return sr, si

# ── Assert exact table (n = 1..30) ──
for n in range(1, 31):
    a, b, c, d = expected_6[n]
    exp_re = a / 2;  exp_im = b * sqrt3 / 2
    exp_dr = c / 2;  exp_di = d * sqrt3 / 2
    if n == 1:
        act_re = act_im = act_dr = act_di = 0.0
    else:
        act_re, act_im, _, _ = compute_numeric(n, To6, spf)
        act_dr, act_di = compute_sdot6(n)
    assert abs(act_re - exp_re) < EPS, f"n={n}: Re(s)={act_re}, want {exp_re}"
    assert abs(act_im - exp_im) < EPS, f"n={n}: Im(s)={act_im}, want {exp_im}"
    assert abs(act_dr - exp_dr) < EPS, f"n={n}: Re(ṡ)={act_dr}, want {exp_dr}"
    assert abs(act_di - exp_di) < EPS, f"n={n}: Im(ṡ)={act_di}, want {exp_di}"
print("✓ All 30 × 4 = 120 exact table values verified for T_o = 6\n")

# ── Assert lattice structure for n = 2..1000 ──
N6 = 1000
for n in range(2, N6 + 1):
    sr, si, _, _ = compute_numeric(n, To6, spf)
    dr, di = compute_sdot6(n)
    for label, re, im in [("s_n", sr, si), ("ṡ_n", dr, di)]:
        a = round(2 * re)
        b = round(2 * im / sqrt3)
        assert abs(re - a / 2) < EPS, f"n={n}: Re({label}) not on lattice"
        assert abs(im - b * sqrt3 / 2) < EPS, f"n={n}: Im({label}) not on lattice"

print(f"✓ Lattice a/2 + i·b√3/2 verified for ALL n = 1..{N6}  (s_n and ṡ_n)")
```

```
✓ All 30 × 4 = 120 exact table values verified for T_o = 6

✓ Lattice a/2 + i·b√3/2 verified for ALL n = 1..1000  (s_n and ṡ_n)
```

## $T_o = 6$: Sections 1-3 — Angles, Classes, and Symmetries

### 1. $\omega^p$ depends only on $p \bmod 6$

At $T_o = 6$: $\omega = e^{2\pi i/6}$, so $\omega^p = \omega^{p \bmod 6}$.

### 2. Prime-angle classes

| Residue $p \bmod 6$ | $\theta/2\pi$ | Degrees | $\omega^r$ | Primes |
|:--------------------:|:---------:|:-------:|:----------:|:-------|
| 1 | $1/6$ | $60°$ | $\tfrac{1}{2}+i\tfrac{\sqrt3}{2}$ | $7, 13, 19, 31, 37, \ldots$ |
| 2 | $2/6$ | $120°$ | $-\tfrac{1}{2}+i\tfrac{\sqrt3}{2}$ | $2$ only |
| 3 | $3/6$ | $180°$ | $-1$ | $3$ only |
| 5 | $5/6$ | $300°$ | $\tfrac{1}{2}-i\tfrac{\sqrt3}{2}$ | $5, 11, 17, 23, 29, \ldots$ |

For primes $p \ge 5$: only **two** residue classes, $\{1, 5\}$.

### 3. Symmetries

- **Conjugate pair**: $\omega^1$ and $\omega^5$ are conjugates ($60° + 300° = 360°$)
- **$\omega^3 = -1$**: so $\omega^{a+3} = -\omega^a$ (antipodal on the circle)
- **Antipodal pairs** $(d, d{+}3 \bmod 6)$: only $\{2, 5\}$ is viable among prime residues

```python
# ── Verify angles, conjugate pairs, antipodal cancellation at T_o=6 ──
import cmath
w6 = cmath.exp(2j * math.pi / To6)

# §1: ω^p = ω^{p mod 6} for all primes
angle_by_digit6 = {}
for p in range(2, 1000):
    if not is_prime(p, spf):
        continue
    d = p % To6
    theta_mod = (2 * math.pi * p / To6) % (2 * math.pi)
    expected = 2 * math.pi * d / To6
    assert abs(theta_mod - expected) < EPS or abs(theta_mod - expected - 2*math.pi) < EPS
    angle_by_digit6.setdefault(d, []).append(p)

print(f"Prime residues mod 6 appearing (p < 1000): {sorted(angle_by_digit6.keys())}")
for d in sorted(angle_by_digit6.keys()):
    first_few = angle_by_digit6[d][:6]
    print(f"  digit {d}: {first_few}{'...' if len(angle_by_digit6[d]) > 6 else ''}")
print(f"\n✓ All {sum(len(v) for v in angle_by_digit6.values())} primes verified: ω^p = ω^(p mod 6)\n")

# §3: Conjugate pair
assert abs(w6**1 - (w6**5).conjugate()) < EPS
print(f"  ω^1 = {w6**1:.6f},  ω^5 = {w6**5:.6f}  ← conjugate pair ✓")

# ω^3 = -1
assert abs(w6**3 - (-1)) < EPS
print(f"  ω^3 = {w6**3:.6f} = -1  ✓")

# Antipodal: ω^{a+3} = -ω^a
for a in range(6):
    assert abs(w6**((a + 3) % 6) + w6**a) < EPS
print(f"  ω^{{a+3}} = -ω^a for all a ∈ {{0..5}}  ✓\n")

# Antipodal cancellation pairs
print("  Antipodal pairs (d, d+3 mod 6):")
for d in range(3):
    d2 = (d + 3) % 6
    res = w6**d + w6**d2
    assert abs(res) < EPS
    viable = "✓ VIABLE" if {d, d2} <= {1, 2, 3, 5} else "✗ non-prime residue"
    print(f"    ({d},{d2}):  ω^{d} + ω^{d2} = 0   {viable}")
print("\n  → Among prime residues {1,2,3,5}, the ONLY viable antipodal pair is (2, 5).")
```

```
Prime residues mod 6 appearing (p < 1000): [1, 2, 3, 5]
  digit 1: [7, 13, 19, 31, 37, 43]...
  digit 2: [2]
  digit 3: [3]
  digit 5: [5, 11, 17, 23, 29, 41]...

✓ All 168 primes verified: ω^p = ω^(p mod 6)

  ω^1 = 0.500000+0.866025j,  ω^5 = 0.500000-0.866025j  ← conjugate pair ✓
  ω^3 = -1.000000+0.000000j = -1  ✓
  ω^{a+3} = -ω^a for all a ∈ {0..5}  ✓

  Antipodal pairs (d, d+3 mod 6):
    (0,3):  ω^0 + ω^3 = 0   ✗ non-prime residue
    (1,4):  ω^1 + ω^4 = 0   ✗ non-prime residue
    (2,5):  ω^2 + ω^5 = 0   ✓ VIABLE

  → Among prime residues {1,2,3,5}, the ONLY viable antipodal pair is (2, 5).
```

### 4. How $s_n$ respects mod-6 structure

Since $\omega^{b_k} = \omega^{b_k \bmod 6}$, the value of $s_n$ depends only on the **multiset of residues mod 6** of the prime factors of $n$.

### 5. Dirichlet's theorem at mod 6

For primes $p \ge 5$, only two residue classes exist: $p \equiv 1 \pmod{6}$ and $p \equiv 5 \pmod{6}$.

By Dirichlet's theorem, each contains $\frac{1}{\varphi(6)} = \frac{1}{2}$ of the primes — so the two angle-rays at $\pm 60°$ receive **equal density**.

```python
# §4: s_n structure — examples
# n = 10 = 2 × 5  → residues mod 6: [2, 5]  → s_10 = ω² + ω⁵ = 0 (antipodal)
sn10_6 = compute_numeric(10, To6, spf)
assert abs(sn10_6[0]) < EPS and abs(sn10_6[1]) < EPS
print(f"n = 10 = 2 × 5  → residues mod 6: [2, 5]")
print(f"  s_10 = ω² + ω⁵ = 0  ✓  (antipodal pair)\n")

# n = 105 = 3 × 5 × 7  → residues mod 6: [3, 5, 1]
sn105_6 = compute_numeric(105, To6, spf)
assert abs(sn105_6[0]) < EPS and abs(sn105_6[1]) < EPS
print(f"n = 105 = 3 × 5 × 7  → residues mod 6: [3, 5, 1]")
print(f"  s_105 = ω³ + ω⁵ + ω¹ = ω(1 + ω² + ω⁴) = ω · 0 = 0  ✓  (rotated 3rd roots)\n")

# n = 91 = 7 × 13  → residues [1, 1]
# n = 35 = 5 × 7   → residues [5, 1]    ← same multiset as {digits} differ!
sn91_6 = compute_numeric(91, To6, spf)
sn35_6 = compute_numeric(35, To6, spf)
print(f"n = 91 = 7 × 13  → residues mod 6: [1, 1]")
print(f"  s_91 = 2ω¹ = ({sn91_6[0]:.6f}, {sn91_6[1]:.6f})")
print(f"n = 35 = 5 × 7   → residues mod 6: [5, 1]")
print(f"  s_35 = ω⁵ + ω¹ = ({sn35_6[0]:.6f}, {sn35_6[1]:.6f})")
print(f"  s_91 ≠ s_35  (different digit multisets at T_o=6)\n")

# §5: Dirichlet equidistribution
counts6 = {1: 0, 5: 0}
for p in range(5, 1000):
    if is_prime(p, spf):
        d = p % 6
        if d in counts6:
            counts6[d] += 1
total6 = sum(counts6.values())
print(f"Primes 5 ≤ p < 1000 by residue mod 6:\n")
for d in [1, 5]:
    frac = counts6[d] / total6
    bar = '█' * int(frac * 80)
    print(f"  residue {d}: {counts6[d]:>3} primes  ({frac:.1%})  {bar}")
    assert 0.40 < frac < 0.60, f"residue {d}: fraction {frac} out of range"
print(f"\n  Total: {total6} primes")
print(f"✓ Verified approximate equidistribution (each ≈ 50%)")
```

```
n = 10 = 2 × 5  → residues mod 6: [2, 5]
  s_10 = ω² + ω⁵ = 0  ✓  (antipodal pair)

n = 105 = 3 × 5 × 7  → residues mod 6: [3, 5, 1]
  s_105 = ω³ + ω⁵ + ω¹ = ω(1 + ω² + ω⁴) = ω · 0 = 0  ✓  (rotated 3rd roots)

n = 91 = 7 × 13  → residues mod 6: [1, 1]
  s_91 = 2ω¹ = (1.000000, 1.732051)
n = 35 = 5 × 7   → residues mod 6: [5, 1]
  s_35 = ω⁵ + ω¹ = (1.000000, -0.000000)
  s_91 ≠ s_35  (different digit multisets at T_o=6)

Primes 5 ≤ p < 1000 by residue mod 6:

  residue 1:  80 primes  (48.2%)  ██████████████████████████████████████
  residue 5:  86 primes  (51.8%)  █████████████████████████████████████████

  Total: 166 primes
✓ Verified approximate equidistribution (each ≈ 50%)
```

### 6. Complete census of $s_n = 0$ at $T_o = 6$ ($n \le 1000$)

#### The cancellation mechanism

Since $\omega^3 = -1$, we have $\omega^{a+3} = -\omega^a$, so:
$$\omega^a + \omega^{a+3} = 0 \qquad \text{(antipodal)}$$

Among prime residues mod 6: $\{1, 2, 3, 5\}$, the **only viable antipodal pair** is $\{2, 5\}$.

#### General zero-sum condition

Writing $a_d$ = count of prime factors with residue $d$ mod 6, $s_n = 0$ iff:

$$a_1 = a_3, \qquad a_5 = a_1 + a_2$$

This gives exactly **two atomic mechanisms**:
- **Pairwise (Type A)**: $\{2, 5\}$ — antipodal via $\omega^3 = -1$
- **3rd roots (Type B)**: $\{1, 3, 5\}$ — since $\omega^1 + \omega^3 + \omega^5 = \omega(1 + \omega^2 + \omega^4) = 0$

Every $s_n = 0$ decomposes into copies of these two atomic sets.

```python
# ── Find ALL n ≤ 1000 with s_n = 0 at T_o = 6 ──
zeros6 = []
for n in range(2, N6 + 1):
    sr, si, _, _ = compute_numeric(n, To6, spf)
    if abs(sr) < EPS and abs(si) < EPS:
        zeros6.append(n)

print(f"Found {len(zeros6)} values of n ≤ {N6} with s_n = 0 at T_o = 6.\n")

# Classify: Type A (pure pairwise 2↔5) vs Type B (involves 3rd roots)
typeA6 = []
typeB6 = []
for n in zeros6:
    fs = expand_factors(n)
    digits6 = [p % 6 for p in fs]
    dcnt = Counter(digits6)
    # Pure pairwise: only digits 2 and 5, equal counts
    if set(dcnt.keys()) <= {2, 5} and dcnt.get(2, 0) == dcnt.get(5, 0):
        typeA6.append(n)
    else:
        typeB6.append(n)

print(f"  Type A (pairwise 2↔5):  {len(typeA6)} cases")
print(f"  Type B (involves 3rd roots / mixed): {len(typeB6)} cases\n")

# ── Type A table (first 30) ──
show_A = typeA6[:30]
print(f"{'─'*70}")
print(f"TYPE A: Pairwise (2↔5) cancellation  ({len(typeA6)} total, showing first {len(show_A)})")
print(f"{'─'*70}")
print(f'{"n":>6}  {"factorization":<22} {"digits mod 6":>16}  {"s_n":>6}  {"ṡ_n":>6}  mechanism')
print(f'{"─"*6}  {"─"*22} {"─"*16}  {"─"*6}  {"─"*6}  {"─"*20}')

for n in show_A:
    fs_pe = factorize(n, spf)
    parts = [f"{p}^{e}" if e > 1 else str(p) for p, e in fs_pe]
    factors_str = " × ".join(parts)
    fs = expand_factors(n)
    digits6 = [p % 6 for p in fs]
    dstr = ",".join(str(d) for d in digits6)
    sr, si, _, _ = compute_numeric(n, To6, spf)
    dr, di = compute_sdot6(n)
    assert abs(sr) < EPS and abs(si) < EPS
    sn_sym = "= 0"
    sdot_sym = "= 0" if (abs(dr) < EPS and abs(di) < EPS) else "≠ 0"
    pairs = min(Counter(digits6).get(2, 0), Counter(digits6).get(5, 0))
    print(f'{n:>6}  = {factors_str:<20} [{dstr:>14}]  {sn_sym:>6}  {sdot_sym:>6}  {pairs}×(ω²+ω⁵)=0')

print(f"\n✓ All {len(typeA6)} Type A zeros verified")
```

```
Found 76 values of n ≤ 1000 with s_n = 0 at T_o = 6.

  Type A (pairwise 2↔5):  58 cases
  Type B (involves 3rd roots / mixed): 18 cases

──────────────────────────────────────────────────────────────────────
TYPE A: Pairwise (2↔5) cancellation  (58 total, showing first 30)
──────────────────────────────────────────────────────────────────────
     n  factorization              digits mod 6     s_n     ṡ_n  mechanism
──────  ────────────────────── ────────────────  ──────  ──────  ────────────────────
    10  = 2 × 5                [           2,5]     = 0     = 0  1×(ω²+ω⁵)=0
    22  = 2 × 11               [           2,5]     = 0     = 0  1×(ω²+ω⁵)=0
    34  = 2 × 17               [           2,5]     = 0     = 0  1×(ω²+ω⁵)=0
    46  = 2 × 23               [           2,5]     = 0     = 0  1×(ω²+ω⁵)=0
    58  = 2 × 29               [           2,5]     = 0     = 0  1×(ω²+ω⁵)=0
    82  = 2 × 41               [           2,5]     = 0     = 0  1×(ω²+ω⁵)=0
    94  = 2 × 47               [           2,5]     = 0     = 0  1×(ω²+ω⁵)=0
   100  = 2^2 × 5^2            [       2,2,5,5]     = 0     = 0  2×(ω²+ω⁵)=0
   106  = 2 × 53               [           2,5]     = 0     = 0  1×(ω²+ω⁵)=0
   118  = 2 × 59               [           2,5]     = 0     = 0  1×(ω²+ω⁵)=0
   142  = 2 × 71               [           2,5]     = 0     = 0  1×(ω²+ω⁵)=0
   166  = 2 × 83               [           2,5]     = 0     = 0  1×(ω²+ω⁵)=0
   178  = 2 × 89               [           2,5]     = 0     = 0  1×(ω²+ω⁵)=0
   202  = 2 × 101              [           2,5]     = 0     = 0  1×(ω²+ω⁵)=0
   214  = 2 × 107              [           2,5]     = 0     = 0  1×(ω²+ω⁵)=0
   220  = 2^2 × 5 × 11         [       2,2,5,5]     = 0     ≠ 0  2×(ω²+ω⁵)=0
   226  = 2 × 113              [           2,5]     = 0     = 0  1×(ω²+ω⁵)=0
   262  = 2 × 131              [           2,5]     = 0     = 0  1×(ω²+ω⁵)=0
   274  = 2 × 137              [           2,5]     = 0     = 0  1×(ω²+ω⁵)=0
   298  = 2 × 149              [           2,5]     = 0     = 0  1×(ω²+ω⁵)=0
   334  = 2 × 167              [           2,5]     = 0     = 0  1×(ω²+ω⁵)=0
   340  = 2^2 × 5 × 17         [       2,2,5,5]     = 0     ≠ 0  2×(ω²+ω⁵)=0
   346  = 2 × 173              [           2,5]     = 0     = 0  1×(ω²+ω⁵)=0
   358  = 2 × 179              [           2,5]     = 0     = 0  1×(ω²+ω⁵)=0
   382  = 2 × 191              [           2,5]     = 0     = 0  1×(ω²+ω⁵)=0
   394  = 2 × 197              [           2,5]     = 0     = 0  1×(ω²+ω⁵)=0
   454  = 2 × 227              [           2,5]     = 0     = 0  1×(ω²+ω⁵)=0
   460  = 2^2 × 5 × 23         [       2,2,5,5]     = 0     ≠ 0  2×(ω²+ω⁵)=0
   466  = 2 × 233              [           2,5]     = 0     = 0  1×(ω²+ω⁵)=0
   478  = 2 × 239              [           2,5]     = 0     = 0  1×(ω²+ω⁵)=0

✓ All 58 Type A zeros verified
```

```python
# ─── DETAILED BREAKDOWNS at T_o = 6 ───

# n = 10 = 2 × 5
print("▸ n = 10 = 2 × 5")
print("  Residues mod 6: [2, 5]")
print("  s_10 = ω² + ω⁵ = ω²(1 + ω³) = ω²(1 + (-1)) = 0  ✓")
sr, si, _, _ = compute_numeric(10, To6, spf)
dr, di = compute_sdot6(10)
assert abs(sr) < EPS and abs(si) < EPS
assert abs(dr) < EPS and abs(di) < EPS
print("  → s_10 = ṡ_10 = 0  ✓\n")

# n = 100 = 2² × 5²
print("▸ n = 100 = 2² × 5²")
print("  b_k = [2,2,5,5],  d_k = [2,5],  e_k = [2,2]")
print("  Residues mod 6: [2,2,5,5]")
print("  s_100 = 2ω² + 2ω⁵ = 2(ω² + ω⁵) = 0  ✓")
sr, si, _, _ = compute_numeric(100, To6, spf)
dr, di = compute_sdot6(100)
assert abs(sr) < EPS and abs(si) < EPS
assert abs(dr) < EPS and abs(di) < EPS
print("  → s_100 = ṡ_100 = 0  ✓\n")

# n = 110 = 2 × 5 × 11;  11 mod 6 = 5
print("▸ n = 110 = 2 × 5 × 11")
print("  Residues mod 6: [2, 5, 5]  — two 5's but only one 2")
sr, si, _, _ = compute_numeric(110, To6, spf)
print(f"  s_110 = ω² + ω⁵ + ω⁵ = ω² + 2ω⁵ = ({sr:.6f}, {si:.6f}) ≠ 0")
assert not (abs(sr) < EPS and abs(si) < EPS)
print("  → Unbalanced → s_110 ≠ 0  ✓\n")

# n = 220 = 2² × 5 × 11;  digits 6: [2,2,5,5]
print("▸ n = 220 = 2² × 5 × 11")
print("  Residues mod 6: [2, 2, 5, 5]  — balanced!")
sr, si, _, _ = compute_numeric(220, To6, spf)
dr, di = compute_sdot6(220)
assert abs(sr) < EPS and abs(si) < EPS
sdot_zero_220 = abs(dr) < EPS and abs(di) < EPS
print(f"  s_220 = 2ω² + 2ω⁵ = 2(ω²+ω⁵) = 0  ✓")
print(f"  ṡ_220 = ω² + ω⁵ + ω^11 = ω² + 2ω⁵ = ({dr:.6f}, {di:.6f}) {'= 0' if sdot_zero_220 else '≠ 0'}")
if not sdot_zero_220:
    print("  → s_220 = 0 but ṡ_220 ≠ 0  (two distinct primes 5,11 share residue 5)")
print()

# ── ṡ_n analysis ──
print("─── WHEN DOES ṡ_n ALSO VANISH? (T_o = 6) ───\n")
sdot_zero6 = []
sdot_nonzero6 = []
for n in zeros6:
    dr, di = compute_sdot6(n)
    if abs(dr) < EPS and abs(di) < EPS:
        sdot_zero6.append(n)
    else:
        sdot_nonzero6.append(n)

print(f"Of the {len(zeros6)} zeros (s_n = 0):")
print(f"  ṡ_n = 0 as well:  {len(sdot_zero6)} cases")
print(f"  ṡ_n ≠ 0:          {len(sdot_nonzero6)} cases\n")

if sdot_nonzero6:
    print("Cases where s_n = 0 but ṡ_n ≠ 0 (first 15):")
    for n in sdot_nonzero6[:15]:
        fs_pe = factorize(n, spf)
        parts = [f"{p}^{e}" if e > 1 else str(p) for p, e in fs_pe]
        fstr = " × ".join(parts)
        dr, di = compute_sdot6(n)
        dd = sorted([p % 6 for p, e in fs_pe])
        print(f"  n = {n:>5} = {fstr:<20}  d_k mod 6={dd}  ṡ=({dr:.4f},{di:.4f})")
    if len(sdot_nonzero6) > 15:
        print(f"  ... and {len(sdot_nonzero6) - 15} more")
    print()

print("Key insight: ṡ_n ≠ 0 happens when distinct primes share a residue mod 6")
print("(e.g. 5 and 11 both ≡ 5 mod 6), breaking the pairing.")
```

```
▸ n = 10 = 2 × 5
  Residues mod 6: [2, 5]
  s_10 = ω² + ω⁵ = ω²(1 + ω³) = ω²(1 + (-1)) = 0  ✓
  → s_10 = ṡ_10 = 0  ✓

▸ n = 100 = 2² × 5²
  b_k = [2,2,5,5],  d_k = [2,5],  e_k = [2,2]
  Residues mod 6: [2,2,5,5]
  s_100 = 2ω² + 2ω⁵ = 2(ω² + ω⁵) = 0  ✓
  → s_100 = ṡ_100 = 0  ✓

▸ n = 110 = 2 × 5 × 11
  Residues mod 6: [2, 5, 5]  — two 5's but only one 2
  s_110 = ω² + ω⁵ + ω⁵ = ω² + 2ω⁵ = (0.500000, -0.866025) ≠ 0
  → Unbalanced → s_110 ≠ 0  ✓

▸ n = 220 = 2² × 5 × 11
  Residues mod 6: [2, 2, 5, 5]  — balanced!
  s_220 = 2ω² + 2ω⁵ = 2(ω²+ω⁵) = 0  ✓
  ṡ_220 = ω² + ω⁵ + ω^11 = ω² + 2ω⁵ = (0.500000, -0.866025) ≠ 0
  → s_220 = 0 but ṡ_220 ≠ 0  (two distinct primes 5,11 share residue 5)

─── WHEN DOES ṡ_n ALSO VANISH? (T_o = 6) ───

Of the 76 zeros (s_n = 0):
  ṡ_n = 0 as well:  69 cases
  ṡ_n ≠ 0:          7 cases

Cases where s_n = 0 but ṡ_n ≠ 0 (first 15):
  n =   220 = 2^2 × 5 × 11          d_k mod 6=[2, 5, 5]  ṡ=(0.5000,-0.8660)
  n =   340 = 2^2 × 5 × 17          d_k mod 6=[2, 5, 5]  ṡ=(0.5000,-0.8660)
  n =   460 = 2^2 × 5 × 23          d_k mod 6=[2, 5, 5]  ṡ=(0.5000,-0.8660)
  n =   580 = 2^2 × 5 × 29          d_k mod 6=[2, 5, 5]  ṡ=(0.5000,-0.8660)
  n =   748 = 2^2 × 11 × 17         d_k mod 6=[2, 5, 5]  ṡ=(0.5000,-0.8660)
  n =   820 = 2^2 × 5 × 41          d_k mod 6=[2, 5, 5]  ṡ=(0.5000,-0.8660)
  n =   940 = 2^2 × 5 × 47          d_k mod 6=[2, 5, 5]  ṡ=(0.5000,-0.8660)

Key insight: ṡ_n ≠ 0 happens when distinct primes share a residue mod 6
(e.g. 5 and 11 both ≡ 5 mod 6), breaking the pairing.
```

### 6b. Type B: 3rd-root-of-unity cancellations ($T_o = 6$)

Beyond pairwise $\{2, 5\}$, the identity:

$$\omega^1 + \omega^3 + \omega^5 = \omega(1 + \omega^2 + \omega^4) = 0$$

(rotated 3rd roots of unity) provides a second cancellation mechanism.

This requires one prime factor from each of the residue classes $\{1, 3, 5\} \bmod 6$:
- Residue 1: primes $7, 13, 19, 31, \ldots$
- Residue 3: only prime $3$
- Residue 5: primes $5, 11, 17, 23, \ldots$

Unlike $T_o = 10$ where Type B needs $n \ge 21945$, here the smallest is $n = 3 \times 5 \times 7 = 105$ — well within our scan range!

```python
# ── Verify 3rd-root identity ──
third_root_sum = w6**1 + w6**3 + w6**5
assert abs(third_root_sum) < EPS
print("✓ ω¹ + ω³ + ω⁵ = 0  (rotated 3rd roots of unity)\n")

# Also verify: 1 + ω² + ω⁴ = 0 (standard 3rd roots)
std_third = 1 + w6**2 + w6**4
assert abs(std_third) < EPS
print("✓ 1 + ω² + ω⁴ = 0  (standard 3rd roots)\n")

# ── Type B examples ──
print("─── TYPE B EXAMPLES (3rd-root cancellations) ───\n")
typeB6_examples = []
for n in typeB6:
    fs = expand_factors(n)
    digits6 = [p % 6 for p in fs]
    dcnt = Counter(digits6)
    typeB6_examples.append((n, dcnt))

# Show first 20 Type B zeros
print(f'{"n":>6}  {"factorization":<28} {"digits mod 6":>18}  {"decomposition"}')
print(f'{"─"*6}  {"─"*28} {"─"*18}  {"─"*30}')
for n, dcnt in typeB6_examples[:20]:
    fs_pe = factorize(n, spf)
    parts = [f"{p}^{e}" if e > 1 else str(p) for p, e in fs_pe]
    fstr = " × ".join(parts)
    fs = expand_factors(n)
    digs = [p % 6 for p in fs]
    dstr = ",".join(str(d) for d in digs)
    
    # Decompose into atomic sets
    a1 = dcnt.get(1, 0)
    a2 = dcnt.get(2, 0)
    a3 = dcnt.get(3, 0)
    a5 = dcnt.get(5, 0)
    # From the zero-sum condition: a1 = a3, a5 = a1 + a2
    # So: k = a1 = a3 copies of {1,3,5} and m = a2 copies of {2,5}
    k = a1  # = a3
    m = a2
    decomp_parts = []
    if m > 0:
        decomp_parts.append(f"{m}×{{2,5}}" if m > 1 else "{2,5}")
    if k > 0:
        decomp_parts.append(f"{k}×{{1,3,5}}" if k > 1 else "{1,3,5}")
    decomp = " ∪ ".join(decomp_parts)
    
    sr, si, _, _ = compute_numeric(n, To6, spf)
    assert abs(sr) < EPS and abs(si) < EPS
    print(f'{n:>6}  = {fstr:<26} [{dstr:>16}]  {decomp}')

if len(typeB6_examples) > 20:
    print(f"  ... and {len(typeB6_examples) - 20} more Type B zeros")

print(f"\n✓ All {len(typeB6)} Type B zeros verified at T_o = 6")

# ── Verify decomposition holds for ALL Type B zeros ──
for n, dcnt in typeB6_examples:
    a1, a2, a3, a5 = dcnt.get(1,0), dcnt.get(2,0), dcnt.get(3,0), dcnt.get(5,0)
    assert a1 == a3, f"n={n}: a1={a1} ≠ a3={a3}"
    assert a5 == a1 + a2, f"n={n}: a5={a5} ≠ a1+a2={a1+a2}"
print(f"✓ All {len(typeB6)} Type B zeros satisfy a₁=a₃, a₅=a₁+a₂ decomposition")
```

```
✓ ω¹ + ω³ + ω⁵ = 0  (rotated 3rd roots of unity)

✓ 1 + ω² + ω⁴ = 0  (standard 3rd roots)

─── TYPE B EXAMPLES (3rd-root cancellations) ───

     n  factorization                      digits mod 6  decomposition
──────  ──────────────────────────── ──────────────────  ──────────────────────────────
   105  = 3 × 5 × 7                  [           3,5,1]  {1,3,5}
   195  = 3 × 5 × 13                 [           3,5,1]  {1,3,5}
   231  = 3 × 7 × 11                 [           3,1,5]  {1,3,5}
   285  = 3 × 5 × 19                 [           3,5,1]  {1,3,5}
   357  = 3 × 7 × 17                 [           3,1,5]  {1,3,5}
   429  = 3 × 11 × 13                [           3,5,1]  {1,3,5}
   465  = 3 × 5 × 31                 [           3,5,1]  {1,3,5}
   483  = 3 × 7 × 23                 [           3,1,5]  {1,3,5}
   555  = 3 × 5 × 37                 [           3,5,1]  {1,3,5}
   609  = 3 × 7 × 29                 [           3,1,5]  {1,3,5}
   627  = 3 × 11 × 19                [           3,5,1]  {1,3,5}
   645  = 3 × 5 × 43                 [           3,5,1]  {1,3,5}
   663  = 3 × 13 × 17                [           3,1,5]  {1,3,5}
   861  = 3 × 7 × 41                 [           3,1,5]  {1,3,5}
   897  = 3 × 13 × 23                [           3,1,5]  {1,3,5}
   915  = 3 × 5 × 61                 [           3,5,1]  {1,3,5}
   969  = 3 × 17 × 19                [           3,5,1]  {1,3,5}
   987  = 3 × 7 × 47                 [           3,1,5]  {1,3,5}

✓ All 18 Type B zeros verified at T_o = 6
✓ All 18 Type B zeros satisfy a₁=a₃, a₅=a₁+a₂ decomposition
```

```python
# ═══════════════════════════════════════════════════════════════
#  SUMMARY — T_o = 6 vs T_o = 10
# ═══════════════════════════════════════════════════════════════
print("=" * 64)
print("  SUMMARY:  T_o = 6 ANALYSIS")
print("=" * 64)
print(f"""
  T_o = 6:  ω = e^{{2πi/6}},  prime residues mod 6 = {{1, 2, 3, 5}}

  1. ω^p depends only on p mod 6
  2. Primes ≥ 5 have exactly 2 residue classes: {{1, 5}} (vs 4 at T_o=10)
  3. ω¹ ↔ ω⁵ are conjugates (±60°) → real-axis symmetry
  4. ω³ = -1 → antipodal pairs; only (2,5) viable for primes
  5. Dirichlet: equal density in residues 1 and 5 (each ≈ 50%)
  6. s_n = 0 census (n ≤ {N6}):
     • Total zeros:    {len(zeros6)}
     • Type A (2↔5):   {len(typeA6)}   (pairwise antipodal)
     • Type B (mixed):  {len(typeB6)}   (involves {{1,3,5}} 3rd-root sums)
     • ṡ_n = 0 also:   {len(sdot_zero6)}
     • ṡ_n ≠ 0:        {len(sdot_nonzero6)}
  7. ALL zero-sum multisets decompose into copies of
     {{2,5}} and {{1,3,5}} — verified for all {len(zeros6)} zeros.
  8. Lattice: every s_n, ṡ_n lives on a/2 + i·b√3/2 (a,b ∈ ℤ)

  ──── COMPARISON: T_o = 6 vs T_o = 10 ────

  │ Property               │ T_o = 6        │ T_o = 10       │
  │────────────────────────│───────────────│───────────────│
  │ Prime residues (p≥5)   │ {{1, 5}}         │ {{1,3,7,9}}      │
  │ # angle rays (p≥5)     │ 2              │ 4              │
  │ Antipodal pair         │ {{2, 5}}         │ {{2, 7}}         │
  │ Multi-vector mechanism │ {{1,3,5}} 3rd rt │ {{1,3,5,7,9}} 5th│
  │ Smallest Type B        │ 105            │ 21945          │
  │ Zeros ≤ 1000           │ {len(zeros6):<14} │ {len(zeros):<14} │
  │ Type A / Type B split  │ {len(typeA6)}/{len(typeB6):<12} │ {len(type_pairwise)}/0            │
  │ Lattice                │ a/2 + ib√3/2   │ SymVal (9-dim)  │

  ✓ All {len(zeros6)} zeros + lattice + Dirichlet + decompositions asserted.
""")

# Final assertion count
n_asserts = 120 + 2 * N6  # table + lattice
n_asserts += len(zeros6)  # s_n = 0 checks
n_asserts += len(typeB6) * 2  # decomposition checks
print(f"  Total assertions in T_o = 6 analysis: ≥ {n_asserts}")
```

```
================================================================
  SUMMARY:  T_o = 6 ANALYSIS
================================================================

  T_o = 6:  ω = e^{2πi/6},  prime residues mod 6 = {1, 2, 3, 5}

  1. ω^p depends only on p mod 6
  2. Primes ≥ 5 have exactly 2 residue classes: {1, 5} (vs 4 at T_o=10)
  3. ω¹ ↔ ω⁵ are conjugates (±60°) → real-axis symmetry
  4. ω³ = -1 → antipodal pairs; only (2,5) viable for primes
  5. Dirichlet: equal density in residues 1 and 5 (each ≈ 50%)
  6. s_n = 0 census (n ≤ 1000):
     • Total zeros:    76
     • Type A (2↔5):   58   (pairwise antipodal)
     • Type B (mixed):  18   (involves {1,3,5} 3rd-root sums)
     • ṡ_n = 0 also:   69
     • ṡ_n ≠ 0:        7
  7. ALL zero-sum multisets decompose into copies of
     {2,5} and {1,3,5} — verified for all 76 zeros.
  8. Lattice: every s_n, ṡ_n lives on a/2 + i·b√3/2 (a,b ∈ ℤ)

  ──── COMPARISON: T_o = 6 vs T_o = 10 ────

  │ Property               │ T_o = 6        │ T_o = 10       │
  │────────────────────────│───────────────│───────────────│
  │ Prime residues (p≥5)   │ {1, 5}         │ {1,3,7,9}      │
  │ # angle rays (p≥5)     │ 2              │ 4              │
  │ Antipodal pair         │ {2, 5}         │ {2, 7}         │
  │ Multi-vector mechanism │ {1,3,5} 3rd rt │ {1,3,5,7,9} 5th│
  │ Smallest Type B        │ 105            │ 21945          │
  │ Zeros ≤ 1000           │ 76             │ 26             │
  │ Type A / Type B split  │ 58/18           │ 26/0            │
  │ Lattice                │ a/2 + ib√3/2   │ SymVal (9-dim)  │

  ✓ All 76 zeros + lattice + Dirichlet + decompositions asserted.

  Total assertions in T_o = 6 analysis: ≥ 2232
```

## Summary: $T_o = 6$

At $T_o = 6$, the structure is **simpler** than $T_o = 10$:

1. Only **2 angle-rays** for primes $\ge 5$ (vs 4 at $T_o = 10$)
2. The **only antipodal pair** is $\{2, 5\}$ (via $\omega^3 = -1$)
3. **3rd-root cancellations** $\{1, 3, 5\}$ are achievable with small $n$ (smallest $n = 105$)
4. **Every** $s_n = 0$ decomposes into atomic sets $\{2,5\}$ and $\{1,3,5\}$
5. All values live on the **Eisenstein lattice** $\frac{a}{2} + i\frac{b\sqrt{3}}{2}$

> **All zero cases, lattice structure, and decompositions asserted up to $N = 1000$.** $\checkmark$
