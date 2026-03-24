# Taylor Series · sin(x) Visualizer

![Python](https://img.shields.io/badge/Python-3.8%2B-blue?logo=python&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-green)
![Platform](https://img.shields.io/badge/Platform-macOS%20%7C%20Linux%20%7C%20Windows-lightgrey)
![Tkinter](https://img.shields.io/badge/GUI-Tkinter-orange)
![Matplotlib](https://img.shields.io/badge/Visualization-Matplotlib-blueviolet?logo=matplotlib)

An interactive desktop application that visualizes the convergence of the **Taylor series approximation of sin(x)** — with and without quadrant range reduction — in real time.

---

## Screenshot

![App Screenshot](assets/screenshot.png)

> *Left panel: approximation value vs. number of terms. Right panel: absolute error on a log scale — showing how range reduction dramatically accelerates convergence for large x.*

---

## Features

- **Live computation** — adjust x or the number of terms via slider; results update instantly
- **Dual-panel chart**
  - *Convergence panel*: Taylor approximation vs. exact `math.sin(x)` value
  - *Error panel*: absolute error on a **log scale**, comparing raw vs. range-reduced Taylor
- **Range reduction vs. raw comparison** — both methods plotted simultaneously so the difference is visible
- **16-digit precision** output matching Python's `math.sin` float64 resolution
- **Dark UI** with color-coded error magnitude (green / yellow / red)

---

## The Math

### Taylor Series for sin(x)

The Taylor series expansion of sin(x) around 0 is:

$$\sin(x) = \sum_{n=0}^{\infty} \frac{(-1)^n}{(2n+1)!} x^{2n+1} = x - \frac{x^3}{3!} + \frac{x^5}{5!} - \frac{x^7}{7!} + \cdots$$

This series converges for all real x, but **convergence becomes very slow for large |x|** because the terms grow large before cancelling.

### Quadrant Range Reduction

To improve convergence, x is first reduced into the first quadrant **[0, π/2]** using the symmetry properties of sine:

```
x  ←  x mod 2π                   # wrap to [0, 2π)

Quadrant I   [0,   π/2]:  sin(x)  =  +sin(x)
Quadrant II  [π/2, π  ]:  sin(x)  =  +sin(π − x)
Quadrant III [π,   3π/2]: sin(x)  =  −sin(x − π)
Quadrant IV  [3π/2, 2π ]: sin(x)  =  −sin(2π − x)
```

The reduced argument `xr ∈ [0, π/2]` is always small (≤ 1.5708...), so the Taylor series needs only a few terms to reach machine precision — regardless of how large the original x is.

### Why the Error Panel Matters

On a linear scale both methods look nearly flat after n ≈ 3. The log-scale error panel reveals what actually happens:

| x value | Raw Taylor converges at n ≈ | Range-reduced converges at n ≈ |
|---------|----------------------------|-------------------------------|
| 1.0     | 8                          | 5                             |
| 10.0    | 18                         | 5                             |
| 31.0    | never (within n=25)        | 4                             |

---

## Installation

**Requirements:** Python 3.8+, Tkinter (usually bundled), Matplotlib

```bash
# Clone the repository
git clone https://github.com/your-username/taylor-series-sin.git
cd taylor-series-sin

# Install dependencies
pip install matplotlib

# Run
python3 taylor_gui.py
```

> **macOS note:** If Tkinter is missing, install it via Homebrew:
> ```bash
> brew install python-tk
> ```

> **Linux note:**
> ```bash
> sudo apt install python3-tk
> ```

---

## Usage

| Control | Description |
|--------|-------------|
| `x =` input field | Enter any real number; press **Enter** or click **Compute** |
| `terms n =` slider | Drag to change the number of Taylor terms (1–30) |
| **Compute** button | Manually trigger recalculation |

The three result cards show:
- **Taylor** — approximation from `sin_taylor_reduced(x, n)`
- **math.sin** — Python's built-in `math.sin(x)` (reference)
- **Error** — absolute difference, color-coded by magnitude

| Error color | Magnitude |
|-------------|-----------|
| 🟢 Green    | < 1 × 10⁻¹⁰ |
| 🟡 Yellow   | < 1 × 10⁻⁴  |
| 🔴 Red      | ≥ 1 × 10⁻⁴  |

---

## Project Structure

```
taylor-series-sin/
├── taylor_gui.py      # Full GUI application (Tkinter + Matplotlib)
├── taylor.py          # Standalone CLI script
└── README.md
```

---

## CLI Version

A minimal command-line version is also included:

```bash
python3 taylor.py
```

```
Enter value of x: 21

Taylor series sin(21.0) = 0.8366556385360556
math.sin  sin(21.0) = 0.8366556385360560
Error = 3.33e-16
```

---

## License

MIT — free to use, modify, and distribute.
