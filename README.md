> [!TIP] 
> 🦅
> This package is part of the [Eegle.jl](https://github.com/Marco-Congedo/Eegle.jl) ecosystem for EEG data analysis and classification.

> **Loreta** (EEG general library) is a pure-[julia](https://julialang.org/) 100%-human package for computing, testing and using human EEG 
> (Electroencephalography) inverse solutions of the *Minimum Norm* family. Particularly, it implements the following distributed inverse solutions:
>
> - *weighted minimum norm*,
> - *standardized Low-Resolution Electromagnetic Tomography* (sLORETA),
> - *exact Low-Resolution Electromagnetic Tomography* (eLORETA).

For all of them, the *model-driven* and the *data-driven* version are provided, with the latter being actually *beamformers* and being little known in the literature.

> [!NOTE] 
> All mathematical details can be found in the xxx papers (see [References]@ref).
>
> An overview of the formula involved in the implementation is [here](https://github.com/Marco-Congedo/Loreta.jl/blob/master/Documents/Overview.pdf).
>
> Those that are not familiar with the material, may want to start with this [introduction.](https://drive.google.com/file/d0B_albC6Y6I9KczRoNjlsbWxKZ3c/view?resourcekey=0-LJGNC8sOIGlft_FJ565muA)



## 🧭 Index
 [Requirements](#-requirements)

---
## 🧩 Requirements

**Julia**: version ≥ 1.10

---
## 📦 Installation

Execute the following command in julia's REPL:

```julia
    ]add Loreta
```
---

## 🔣 Problem Statement, notation and nomenclature

The **distributed EEG inverse problem** is stated as it follows: we are given an EEG sensor measurement \( v(t) \in \mathbb{R}^{N_e} \) at \(N_e\) electrodes, where \(t\) is time (samples) and \(c\) is mnemonic for 'voltage'; we wish to estimate the current \( c(t) \in \mathbb{R}^{N_v} \) at \(N_v\) cortical grey matter voxels in the three Cartesian spatial directions \((x, y, z)\). We have:

- **Forward equation**, determining the scalp voltage given the current distribution : \(v(t) = K c(c)\)
    It is unique for a given *leadfield matrix* \( K \in \mathbb{R}^{N_e \times (N_v \times 3)} \), which is a physical head model.

- **Inverse solution**, estimating the current distribution given the scalp voltage : \(c(t) = T c(t)\)
    It is not-unique. Each method yields a different *tranfer matrix* \( T \in \mathbb{R}^{(N_v \times 3) \times N_e } \).

A solution is said "genuine" or to "respect the measurement" if \(KT=I\). The weighted minimum norm and eLORETA are genuine solutions, while sLORETA is not.

Matrix \(TK=I\) is named the *resolution matrix*. Its successive groups of three columns, one for each voxel, are named the *point-spread functions*. They allows to ascertain whether the transfer matrix is capable of localizing correctly a single current dipole, regardless its position (voxel) and orientation. This is a minimal localization capability for an inverse solution, as it assumes the absence of noise in the measurement and the existence of only one active dipole at a time. sLORETA and eLORETA do possess this property, while the minimum norm does not.

## 🔌 API

The package exports the following functions:

| Function | Description |
|:---------|:---------|
| [centeringMatrix](@ref)   | common average reference operator (alias: ℌ)   |
| [c2cd](@ref)              | compute the current density vector given a current vector   |
| [psfLocError](@ref)       | point spread function localization error   |
| [psfErrors](@ref)         | point spread function localization, spread and equalization errors |
| [minnorm](@ref)           | minimum norm transfer matrix (model and data-driven) |
| [sLORETA](@ref)           | sLORETA transfer matrix (model and data-driven)|
| [eLORETA](@ref)           | eLORETA transfer matrix (model and data-driven)|


#### centeringMatrix

```julia
function centeringMatrix(N::Int)
```

The common average reference (CAR) operator for referencing EEG data 
potentials so that their mean across sensors (space) is zero at all samples.

Let ``X`` be the ``T×N`` EEG recording, where ``T`` and ``N`` denotes the number of samples and channels (sensors), respectively,
and let ``H_N`` be the ``N×N`` centering matrix, then 

``Y=XH`` 

is the CAR (or *centered*) data.

``H_N`` is named the *common average reference operator*. It is given at p.67 by [Searle1982book](@cite), as

``H_N = I_N - \\frac{1}{N} \\left( \\mathbf{1}_N \\mathbf{1}_N^\\top \\right)``

where ``I_N`` is the N-dimensional identity matrix and ``\\mathbf{1}_N`` is the ``N``-dimensional vector of ones.

**Alias** ℌ (U+0210C, with escape sequence "frakH")

**Return** the ``N×N`` centering matrix.

**See**[`car!`](@ref)

**Examples**
```julia
using Eegle

X = randn(128, 19)

# CAR
X_car = X * centeringMatrix(size(X, 2))
# or
X_car = X * ℌ(size(X, 2))

# double-centered data: zero mean across time and space
X_dc = ℌ(size(X, 1)) * X * ℌ(size(X, 2))
```

[▲ API index](#-api)



## 💡 Examples

The examples here below assume the existence of data ``X\in \mathbb{R}^{T \times N_X}``, sampling rate `sr` and labels `sensors`:



## —͟͟͞͞★ Quick Start

A large collection of [tutorials](https://marco-congedo.github.io/Eegle.jl/stable/Tutorials/) (mostly to come) will get you on track.

---

---
## ✍️ About the authors

[Marco Congedo](https://sites.google.com/site/marcocongedo) is a Research Director of [CNRS](http://www.cnrs.fr/en) (Centre National de la Recherche Scientifique), working at [UGA](https://www.univ-grenoble-alpes.fr/english/) (University of Grenoble Alpes). **Contact**: first name dot last name at gmail dot com.

---
## 🌱 Contribute

Please contact the author if you are interested in contributing.

