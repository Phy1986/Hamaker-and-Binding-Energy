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

`Elements Info/` holds f1 scattering factors and molar masses for 92 elements, used by Alpha.

## Key relations

```
Eh = 40.5 / d^2.5                 Ks = sqrt(4 kF / (pi a_B))
C  = 14.4 b exp(-Ks r0) |ZA-ZB| / r0
Ep = sqrt(Eh^2 + C^2)             
hbar*omega_p = 28.8 sqrt(N / Vm)
eps_inf = 1 + (hbar*omega_g / Ep)^2 (1 - x + x^2/3),  x = Ep / 4Ef
eps(i xi) = 1 + (eps_inf - 1) / (1 + (xi/omega_UV)^alpha)
omega_UV = 3.05 Eg^0.736
H = (3 kB T / 4 pi) sum_n int Li3(r^2) dpsi,  
E_vdW = H / (12 pi D0^2), D0 = 1.66 A       E_total = E_vdW / (1 - f_Pauling)
```

## Requirements

Python 3.10+, pandas, numpy, openpyxl, plus pymatgen (Bandgap) and mendeleev (Ionicity). Developed under 3.12.7.

