
---

**Loreta** (EEG general library) is a pure-[julia](https://julialang.org/) 100%-human package for computing, testing and using human EEG (Electroencephalography) inverse solutions of the *Minimum Norm* family. Particularly, it implements the following distributed inverse solutions:

- the **weighted minimum norm* ,

- the **standardized LowResolution Electromagnetic Tomography** (sLORETA),

- the **exact LowResolution Electromagnetic Tomography** (eLORETA).

For all of them, the **model-driven** and the **data-driven** version are provided, with the latter being actually **beamformers**.

All mathematical details can be found in the xxx papers (see [References]@ref).

Those that are not familiar with the material, may want to start with this [introduction.](https://drive.google.com/file/d/0B_albC6Y6I9KczRoNjlsbWxKZ3c/view?resourcekey=0-LJGNC8sOIGlft_FJ565muA)

An overview is xxx

!!! note
    This package is part of the [Eegle.jl](https://github.com/Marco-Congedo/Eegle.jl) ecosystem.

It is the fundamental brick allowing the integration of several packages dedicated to human EEG.

<img src="docs/src/assets/Fig1_index.png" width="780" style="display: block; margin: auto;">

---
## 🧩 Requirements

**Julia**: version ≥ 1.10

---
## ⚙️ Installation

Execute the following command in julia's REPL:

```julia
    ]add Eegle
```
---

The package is self-contained, as it re-exports several packages and all its submodules. 

## —͟͟͞͞★ Quick Start

A large collection of [tutorials](https://marco-congedo.github.io/Eegle.jl/stable/Tutorials/) (mostly to come) will get you on track.

---
## 🤔 Disclaimer

**Eegle** is currently under development and the full testing process has not been completed. 

---
## ✍️ About the authors

[Marco Congedo](https://sites.google.com/site/marcocongedo), corresponding author, is a Research Director of [CNRS](http://www.cnrs.fr/en) (Centre National de la Recherche Scientifique), working at [UGA](https://www.univ-grenoble-alpes.fr/english/) (University of Grenoble Alpes). **Contact**: first name dot last name at gmail dot com.

[Fahim Doumi](https://www.linkedin.com/in/fahim-doumi-4888a9251/?locale=fr_FR) at the time of writing was a research ingeneer at the Department of Enginnering of the [University Federico II of Naples](https://www.unina.it/en_GB/home).

---
## 🌱 Contribute

Please contact the authors if you are interested in contributing.

---
| 🎓 **Documentation**  | 
|:-------------------------------------:|
| [![](https://img.shields.io/badge/docs-stable-blue.svg)](https://Marco-Congedo.github.io/Eegle.jl/stable) | 
| [![](https://img.shields.io/badge/docs-dev-blue.svg)](https://Marco-Congedo.github.io/Eegle.jl) | 
| [![](https://img.shields.io/badge/tutorials-blue.svg)](https://marco-congedo.github.io/Eegle.jl/stable/Tutorials/) |
