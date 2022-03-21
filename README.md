# LogarithmicNumbers.jl

A [logarithmic number system](https://en.wikipedia.org/wiki/Logarithmic_number_system) for
Julia.

Provides the signed `ULogarithmic` and unsigned `Logarithmic` types for representing real
numbers on a logarithmic scale.

This is useful when numbers are too big or small to fit accurately into a `Float64` and you
only really care about magnitude.

For example, it can be useful to represent probabilities in this form, and you don't need to
worry about getting zero when multiplying many of them together.

## Installation

```
] add LogarithmicNumbers
```

## Example

```julia
julia> using LogarithmicNumbers

julia> ULogarithmic(2.7)
exp(0.9932517730102834)

julia> float(ans)
2.7

julia> x = exp(ULogarithmic, 1000) - exp(ULogarithmic, 998)
exp(999.8545865421312)

julia> float(x) # overflows
Inf

julia> log(x)
999.8545865421312
```

## Documentation

These types are exported:
* `ULogarithmic{T}` represents a non-negative real number by its logarithm of type `T`.
* `Logarithmic{T}` represents a real number by its absolute value as a `ULogarithmic{T}` and
  a sign bit.
* `ULogarithmicF16`, `ULogarithmicF32`, `ULogarithmicF64`, `LogarithmicF16`,
  `LogarithmicF32` and `LogarithmicF64` are type aliases for `ULogarithmic{Float16}` etc.

Features:
* `ULogarithmic(x)` (and `Logarithmic(x)`, etc.) represents the number `x`.
* `exp(ULogarithmic, x)` represents `exp(x)` (and `x` can be huge).
* Arithmetic: `+`, `-`, `*`, `/`, `^`, `inv`, `log`, `prod`, `sum`.
* Comparisons: equality, ordering, `cmp`, `isless`.
* Random: `rand(ULogarithmic)` is a random number in the unit interval.
* Other functions: `float`, `big`, `unsigned` (converts `ULogarithmic` to `Logarithmic`),
  `signed` (vice versa), `widen`, `typemin`, `typemax`, `zero`, `one`, `iszero`, `isone`,
  `isinf`, `isfinite`, `isnan`, `sign`, `signbit`, `abs`, `nextfloat`, `prevfloat`, `write`,
  `read`.

## Interoperability with other packages

### StatsFuns.jl

Calling `normcdf(ULogarithmic, ...)` is like calling `normcdf(...)` but returns the answer
as a `ULogarithmic` (and calls `normlogcdf(...)` internally).

Similarly there is `normpdf(ULogarithmic, ...)` and `normccdf(ULogarithmic, ...)` and
equivalents for other distributions.

### Distributions.jl

Calling `cdf(ULogarithmic, ...)` is like calling `cdf(...)` but returns the answer as a
`ULogarithmic` (and calls `logcdf(...)` internally).

Similarly there is `ccdf(ULogarithmic, ...)` and `pdf(ULogarithmic, ...)`.
