> [!TIP] 
> 🦅
> This package is part of the [Eegle.jl](https://github.com/Marco-Congedo/Eegle.jl) ecosystem for EEG data analysis and classification.

---

**Loreta** (EEG general library) is a pure-[julia](https://julialang.org/) 100%-human package for computing, testing and using human EEG 
(Electroencephalography) inverse solutions of the *Minimum Norm* family. Particularly, it implements the following distributed inverse solutions:
- *weighted minimum norm*,
- *standardized Low-Resolution Electromagnetic Tomography* (sLORETA),
- *exact Low-Resolution Electromagnetic Tomography* (eLORETA).

For all of them, the *model-driven* and the *data-driven* version are provided, with the latter being actually *beamformers* and being little known in the literature.

> [!NOTE] 
> All mathematical details can be found in the xxx papers (see [References]@ref).
>
> An overview of the formula involved in the implementation is [here](https://github.com/Marco-Congedo/Loreta.jl/blob/master/Documents/Overview.pdf).
>
> Those that are not familiar with the material, may want to start with this [introduction.](https://drive.google.com/file/d0B_albC6Y6I9KczRoNjlsbWxKZ3c/view?resourcekey=0-LJGNC8sOIGlft_FJ565muA)

---

## 🧭 Index

- 📦 [Installation](#-installation)
- 🔣 [Problem Statement, Notation and Nomenclature](#-problem-statement-notation-and-nomenclature)
- 🔌 [API](#-api)
- 💡 [Examples](#-examples)
- ✍️ [About the author](#️-about-the-author)
- 🌱 [Contribute](#-contribute)

---

## 📦 Installation

Execute the following command in julia's REPL:

```julia
]add Loreta
```

There is virtually no requirement for this package. Any Julia version starting at 0.7 would work.

[▲index](#-index)

---

## 🔣 Problem Statement, Notation and Nomenclature

We are given an EEG sensor potentials measurement

x(t) ∈ ℝⁿ 

at n electrodes referenced to the common average, in μV units, where t is time (samples);

we wish to estimate the current density

j(t) ∈ ℝᵖ 

at p cortical grey matter voxels, in A/m² units, in the three Cartesian spatial directions (x, y, z)

We have therefore:

**Forward equation** — determining the scalp voltage given the current distribution:

x(t) = K c(t).

It is unique for a given leadfield matrix 

K ∈ ℝⁿ×³ᵖ.

The leadfield encapsulate a physical head model.

**Inverse solution** — estimating the current distribution given the scalp voltage:

j(t) = T x(t).

It is not unique. Each inverse solution method yields a different transfer matrix

T ∈ ℝ³ᵖ×ⁿ.

> [!NOTE] 
> A solution is said *genuine* or to *respect the measurement* if 
> K T = I.
> The weighted minimum norm and eLORETA are genuine solutions, while sLORETA is not.

> [!NOTE] 
> Matrix 
> T K ≠ I 
> is called the resolution matrix. Its successive groups of three columns, one group per voxel, are called the point-spread functions. 
> They allow one to ascertain whether the transfer matrix is capable of correctly localizing a single current dipole, regardless of its position (voxel) and orientation.
>
> This is a minimal localization capability for an inverse solution, as it (unrealistically) assumes the absence of noise in the measurement and the existence of only one active dipole at a time. Nonetheless, it is a minimal requirement. sLORETA and eLORETA possess this property, while the minimum norm does not, like most inverse solution methods found in the literature.

> [!WARNING] 
> Throughout this documentation and in the package it is always assumed both the input data and the leadfield matrix is referenced to the common average --- see [centeringMatrix](#centeringmatrix).

[▲index](#-index)

## 🔌 API

The package exports the following functions:

| Function | Description |
|:---------|:---------|
| [centeringMatrix](@ref)   | common average reference operator (alias: ℌ)   |
| [c2cd](@ref)              | compute the squared magnitude of the current density given a current density vector |
| [psfLocError](@ref)       | point spread function localization error   |
| [psfErrors](@ref)         | point spread function localization, spread and equalization errors |
| [minnorm](@ref)           | compute minimum norm transfer matrix (model and data-driven) |
| [sLORETA](@ref)           | compute sLORETA transfer matrix (model and data-driven)|
| [eLORETA](@ref)           | compute eLORETA transfer matrix (model and data-driven)(by an iterative algorithm)|

[▲index](#-index)

#### centeringMatrix

```julia
function centeringMatrix(N::Int)
```
The common average reference (CAR) operator for referencing EEG data potentials so that their mean across sensors (space) is zero at all samples.

Let X be the T×N EEG recording, where T and N denote the number of samples and channels (sensors), respectively, and let Hₙ be the N×N centering matrix, then

Y = X Hₙ

is the CAR (or centered) data.

Hₙ is named the common average reference operator. It is given at p.67 by Searle (1982)[^1], as

Hₙ = Iₙ − (1/N) (1ₙ 1ₙᵀ)

where Iₙ is the N-dimensional identity matrix and 1ₙ is the N-dimensional vector of ones.

Alias ℌ (U+210C, with escape sequence "frakH")

Return the N×N centering matrix.

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

[▲index](#-index)

---

#### cd2sm

```julia
function cd2sm(j::Vector{R}) where R<:Real
```

'current density to squared magnitude'. 

Return the current density squared magnitude vector comprised of 1/3 of the elements of the input current density vector j. The current density vector j holds successively the triplets (x, y, z). Return the successive sums (x²+y²+z²) for each triplet.

The input vector j may contain any exact multiple of 3 number of elements.

> [!NOTE] 
> Typically, the squared magnitude of the current density is the quantity of interest in neuroimaging studies.

[▲ API index](#-api)

[▲index](#-index)

---

#### psfLocError

'point spread function Localization Error'

Return the number of localization errors obtained by point spread functions given a leadfield matrix K and a corresponding transformation matrix T --- see 🔣 [here](#-problem-statement-notation-and-nomenclature)

```julia
psfLocError(K::Matrix{R}, T::Matrix{R}) where R<:Real
```

[▲ API index](#-api)

[▲index](#-index)

## 💡 Examples

The examples here below assume the existence of data ``X\in \mathbb{R}^{T \times N_X}``, sampling rate `sr` and labels `sensors`:

[▲index](#-index)

---
## ✍️ About the author

[Marco Congedo](https://sites.google.com/site/marcocongedo) is a Research Director of [CNRS](http://www.cnrs.fr/en) (Centre National de la Recherche Scientifique), working at [UGA](https://www.univ-grenoble-alpes.fr/english/) (University of Grenoble Alpes). **Contact**: first name dot last name at gmail dot com.

[▲index](#-index)

---
## 🌱 Contribute

Please contact the author if you are interested in contributing.

[▲index](#-index)



[^1]: Aristimunha, B., Carrara, I., Guetschel, P., Sedlar, S., Rodrigues, P., Sosulski, J., Narayanan, D., Bjareholt, E., Quentin, B., Schirrmeister, R. T.,Kalunga, E., Darmet, L., Gregoire, C., Abdul Hussain, A., Gatti, R., Goncharenko, V., Thielen, J., Moreau, T., Roy, Y., Jayaram, V., Barachant,A., & Chevallier, S. Mother of all BCI Benchmarks (MOABB), 2023. DOI: 10.5281/zenodo.10034223.