# Hamaker Constants and Interlayer Binding Energies of Transition-Metal Halides

Crystallography-based workflow for computing high-frequency dielectric constants, Hamaker constants, and interlayer binding energies of binary transition-metal halides, without broadband dielectric spectra or first-principles calculations.

Structural data (cell volume, formula units, mean metal-halide bond length) plus one measured optical gap per compound feed a Phillips-Van Vechten-Penn dielectric model, followed by a Lifshitz summation. Binding energies are reported for 28 layered MX2 and MX3 compounds; the 6 non-layered noble-metal monohalides (CuX, AgX) stop at the Hamaker constant, having no van der Waals gap to cleave.

## Layout and run order

Each folder holds one notebook plus its input and output workbooks. Modules feed forward, so run them in this order.

| Folder | Computes | Needs |
|---|---|---|
| `Bandgap/` | Penn gap Ep, ionicity fi, Ef, Ks, Vcell, Z, from CIFs | CIF files |
| `Plasmon Energy/` | valence plasma energy | Bandgap output |
| `Dielectric Constant/` | eps_inf | Bandgap + Plasmon output |
| `Ionicity/` | average Pauling ionicity | element list |
| `Alpha/` | dielectric power-law exponent | eps_inf, optical gap, density |
| `Hamaker&BE/` | H, E_vdW, E_total | all of the above |

`Elements Info/` holds f1 scattering factors and molar masses for elements, used by Alpha.

## Key relations

```
Ec  = 40.5 / d_MX^2.5                   Ks  = sqrt(4 kF / (pi a_B))
Ei  = 14.4 b |ZM - ZX| exp(-Ks r0) / r0,   r0 = d_MX / 2
Ep  = sqrt(Ec^2 + Ei^2)
hbar*omega_p0 = 28.8 sqrt(Nval / Vm)
hbar*omega_p^2 = hbar*omega_p0^2 + Ep^2          <- Horie, was missing
eps_inf = 1 + (hbar*omega_p / Ep)^2 (1 - x + x^2/3),  x = Ep / (4 EF)
eps(i xi) = 1 + (eps_inf - 1) / (1 + (xi/omega_UV)^alpha)
hbar*omega_UV = 3.05 Eg^0.736
H = (3 kB T / 2) sum_n' Li3(r^2),  r = (eps(i xi_n) - 1)/(eps(i xi_n) + 1)
E_vdW = H / (12 pi d0^2),  d0 = 1.66 A
E_total = E_vdW / (1 - f),  f = 1 - exp(-dChi^2 / 4)
```

## Requirements

Python 3.10+, pandas, numpy, openpyxl, plus pymatgen (Bandgap) and mendeleev (Ionicity). Developed under 3.12.7.

