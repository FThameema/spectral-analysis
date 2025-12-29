# Spectral Analysis of Fluid Flow

### Overview

I implemented spectral methods to analyze energy distribution across scales in turbulent flows. Using FFT, I computed energy spectra and verified Kolmogorov's k⁻⁵/³ scaling law - a fundamental result in turbulence theory.

### Why Spectral Analysis?

Turbulence has structures at all scales simultaneously. Fourier analysis decomposes velocity into wavelengths (wavenumbers k) showing how energy cascades from large to small scales.

**Kolmogorov's theory** predicts in the inertial range:
```
E(k) ~ ε^(2/3) k^(-5/3)
```
where energy cascades without injection or dissipation.

### Implementation

Used **Fast Fourier Transform** to convert velocity from physical (x,y) to Fourier space (kx,ky):
```python
u_hat = np.fft.fft2(u)
E(k) = ½ ∫ |u_hat(k)|² dk
```

Process: Take FFT → compute energy per mode → bin by radial wavenumber → average over angles (isotropic assumption).

### Energy Cascade

Physical picture: Energy injected at large scales → nonlinear interactions transfer to smaller scales → dissipated at smallest scales by viscosity. The Richardson cascade: "Big whorls have little whorls..."

### Results

[INSERT IMAGE: Energy spectrum E(k) vs k on log-log]

Three regimes:
1. Energy-containing range (low k): injection peak
2. Inertial range (medium k): k⁻⁵/³ power law
3. Dissipation range (high k): exponential decay

[INSERT IMAGE: Compensated spectrum k^(5/3)E(k)]

Flat plateau confirms k⁻⁵/³ scaling in inertial range.

#### Reynolds Number Effects

Low Re (~100): No clear inertial range (scales too close)
High Re (~10⁴): Wide inertial range with clear k⁻⁵/³ scaling

High-Re turbulence is computationally expensive - must resolve from large eddies down to Kolmogorov microscale η.

#### Spectral Methods for PDEs

Also used spectral methods to solve Navier-Stokes directly. In Fourier space, derivatives become multiplication:
```
∂u/∂x → ik u_hat
∇²u → -k² u_hat
```
**Pseudospectral approach:** Nonlinear terms in physical space, derivatives in Fourier space. Much more accurate than finite differences for smooth flows.
**Dealiasing:** Used 2/3 rule to prevent aliasing errors from nonlinear products.

### Validation

Tested on decaying turbulence - energy decays as E(t) ~ t⁻ⁿ. My results match expected decay rate.
<img width="1389" height="489" alt="velocity fluctuations" src="https://github.com/user-attachments/assets/aa54ad6c-8b0d-41ff-82fa-32e637dd20c5" />


### Running the Code

```bash
python spectral_analysis.py
```
Outputs energy spectra, time evolution, velocity fields.

### Key Insights

Spectral methods give exponential accuracy (vs polynomial for finite differences). k⁻⁵/³ spectrum is universal, emerging from Navier-Stokes without tuning. Limitation: requires periodic boundaries - finite difference/volume methods better for complex geometries.

### Computational Cost

For 3D: N³ grid points, O(N³ log N) per timestep. At high Re, prohibitively expensive - LES models needed.

## Future Work

3D spectral DNS, anisotropic turbulence, higher-order statistics (skewness, intermittency), LES subgrid models.

---

This project deepened my understanding of turbulence physics and multi-scale analysis - crucial for modern CFD research.
