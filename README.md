# Hamaker Constants and Interlayer Binding Energies of Transition-Metal Halides

Crystallography-based workflow for computing high-frequency dielectric constants, Hamaker constants, and interlayer binding energies of binary transition-metal halides, without broadband dielectric spectra or first-principles calculations.

Structural data (cell volume, formula units, mean metal–halogen bond length) plus one measured optical gap per compound feed a Phillips–Van Vechten–Penn dielectric model, followed by a Lifshitz summation. Binding energies are reported for 28 layered MX2 and MX3 compounds; the 6 non-layered noble-metal monohalides (CuX, AgX) stop at the Hamaker constant, having no van der Waals gap to cleave.

## Layout and run order

Each folder holds one notebook plus its input and output workbooks. Modules feed forward, so run them in this order.

| Folder | Computes | Needs |
|---|---|---|
| `Bandgap/` | Penn gap Ep, its covalent and ionic parts Ec and Ei, Phillips ionicity fi = Ei^2/Ep^2, EF, Ks, Vcell, Z | CIF files |
| `Plasmon Energy/` | valence plasma energy hbar*omega_p | Bandgap output |
| `Dielectric Constant/` | eps_inf | Bandgap + Plasmon output |
| `Ionicity/` | Pauling ionicity f | element list |
| `Alpha/` | dielectric power-law exponent alpha | eps_inf, optical gap, density |
| `Hamaker&BE/` | H, E_vdW, E_total | all of the above |

`Elements Info/` holds f1 scattering factors and molar masses for elements, used by Alpha.

Note that two distinct ionicities appear: the Phillips ionicity `fi` from `Bandgap/`, which characterizes the Penn gap, and the Pauling ionicity `f` from `Ionicity/`, which enters `E_total`. Folder names contain spaces and `&`, so quote them in a shell: `cd "Hamaker&BE"`.

## Key relations

Energies in eV, lengths in Å, molar volume in cm³/mol. Equation numbers refer to the Methods of the paper.

```
Ec  = 40.5 / d_MX^2.5                                              (9)
Ei  = 14.4 b |ZM - ZX| exp(-Ks r0) / r0,   r0 = d_MX / 2          (10)
Ep  = sqrt(Ec^2 + Ei^2)                                            (8)

n   = Z Nsp / Vcell,  kF = (3 pi^2 n)^(1/3)                   (11,12)
Ks  = sqrt(4 kF / (pi a_B)),  EF = hbar^2 kF^2 / (2 me)       (13,14)

hbar*omega_p0  = 28.8 sqrt(Nval / Vm),  Vm = NA Vcell / Z       (6,7)
hbar*omega_p^2 = hbar*omega_p0^2 + Ep^2        <- Horie correction (15)

eps_inf = 1 + (hbar*omega_p / Ep)^2 (1 - x + x^2/3),  x = Ep/(4 EF)  (16)

eps(i xi)     = 1 + (eps_inf - 1) / (1 + (xi/omega_UV)^alpha)     (19)
hbar*omega_UV = 3.05 Eg^0.736

H = (3 kB T / 2) sum_n' Li3(r^2),  r = (eps(i xi_n) - 1)/(eps(i xi_n) + 1)
                                                               (20,22)
xi_n = 2 pi n kB T,  T = 300 K,  truncated at 300 eV              (21)

E_vdW   = H / (12 pi d0^2),  d0 = 1.66 A                          (23)
E_total = E_vdW / (1 - f),   f = 1 - exp(-dChi^2 / 4)          (24,25)
```

where `d_MX` is the mean metal–halogen bond length, `a_B = 0.529 A` the Bohr radius,
`Li3` the trilogarithm, `sum_n'` a Matsubara sum with half weight on `n = 0`, and
`dChi` the difference in Pauling electronegativities.

`Nval` is the valence-electron count per formula unit (16 for dihalides, 24 for
trihalides, 18 for the CuX/AgX monohalides) and `Nsp` the sp-only count, equal to
`Nval` except for the monohalides, where `Nsp = 8`.

`b` is the dimensionless screening prefactor: 3.05 for the layered TMHs, 3.20 for
AgCl and AgBr, 1.42 for AgI, CuCl, CuBr and CuI. `ZX = 7`; `ZM` is the formal cation
oxidation state (2 or 3), except for the monohalides, where `ZM = 11` and the
effective core charge `ZM* = sigma ZM` enters Eq. (10) with `sigma = 1.20`–`1.49`.

## Requirements

Python 3.10+, pandas, numpy, openpyxl, plus pymatgen (Bandgap) and mendeleev (Ionicity). Developed under 3.12.7.

## Citation

If you use this code or data, please cite:

> E. Rahmanian, A. Sajedi-Moghaddam, R. Asgari, S. H. Aboutalebi,
> A crystallographic route to Hamaker constants and binding energies in
> transition-metal halides, *submitted*.

## License

Released under the MIT License. See [LICENSE](LICENSE).
