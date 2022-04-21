```@meta
CurrentModule = Lowess
```

# Lowess

Documentation for [Lowess](https://github.com/ayushpatnaikgit/Lowess.jl).

```@index
```

This package includes a pure Julia `lowess` function, which is an implementation 
of the LOWESS smoother (references given at the end of the documentation), along with 
a `lowess_model` function can be used to make corresponding models. 

# Tutorial

In this section we will go through a simple example to see how our model works. Consider the following function. 

```julia 
using Lowess, Plots
xs = 10 .* rand(100)
xs = sort(xs)
ys = sin.(xs) .+ 0.5 * rand(100)

model = lowess_model(xs, ys, 0.2)

us = range(extrema(xs)...; step = 0.1)
vs = model(us)

scatter(xs, ys)
plot!(us, vs, legend=false)
```

The above code creates some random data points sampled out of a sine curve, with some noise added to the ordinates. A model is created using this data with `f = 0.2` (`f` is the parameter which controls the amount of smoothing). 

`us` is a vector of abscissas which lie in the range of the abscissas which were passed as input to the model. To get the predicted smooth values for `us`, we are passing it to the model; the result will be the vector `vs`, the vector of predicted values. 

Finally, we get the scatter plot of the input points, and the smooth plot using `us` and `vs`. The plot looks something like the following. 

![Example Plot](lowess.svg)

# Comparison with [Loess.jl](https://github.com/JuliaStats/Loess.jl)

Our package is an alternative to [Loess.jl](https://github.com/JuliaStats/Loess.jl), which 
exports the more general LOESS predictor. In this section, we benchmark the performance of both the packages on a common dataset and do a comparison in terms of the resources used. 

For our test, we use the example given in the tutorial. For the benchmarks, we use the `BenchmarkTools` package. In the below code, we benchmark the performance of our package code.

```julia
using BenchmarkTools, Lowess
xs = 10 .* rand(100)
xs = sort(xs)
ys = sin.(xs) .+ 0.5 * rand(100)

@benchmark begin
    model = lowess_model(xs, ys, 0.2)
    us = range(extrema(xs)...; step = 0.1)
    vs = model(us)
end
```

The output of the above performance benchmark is given below.

```julia-repl
BenchmarkTools.Trial: 10000 samples with 1 evaluation.
Range (min … max):  47.797 μs …  1.420 ms  ┊ GC (min … max): 0.00% … 95.55%
Time  (median):     58.236 μs              ┊ GC (median):    0.00%
Time  (mean ± σ):   58.842 μs ± 15.965 μs  ┊ GC (mean ± σ):  0.23% ±  0.96%
```

The exact same benchmarking code, but with [Loess.jl](https://github.com/JuliaStats/Loess.jl), is given below. 

```julia
using Loess
@benchmark begin
    model = loess(xs, ys, span=0.5)
    us = range(extrema(xs)...; step = 0.1)
    vs = predict(model, us)
end
```

The output of the above benchmarking test is given below.

```julia-repl
BenchmarkTools.Trial: 6040 samples with 1 evaluation.
 Range (min … max):  574.949 μs …   3.726 ms  ┊ GC (min … max):  0.00% … 64.41%
 Time  (median):     693.659 μs               ┊ GC (median):     0.00%
 Time  (mean ± σ):   825.584 μs ± 515.006 μs  ┊ GC (mean ± σ):  16.85% ± 18.90%
```

For the above test, our package code runs much faster compared to [Loess.jl](https://github.com/JuliaStats/Loess.jl).

# References

[1] Cleveland, W. S. (1979). Robust locally weighted regression and smoothing scatterplots. Journal of the American statistical association, 74(368), 829-836. DOI: 10.1080/01621459.1979.10481038

[2] Cleveland, W. S., & Devlin, S. J. (1988). Locally weighted regression: an approach to regression analysis by local fitting. Journal of the American statistical association, 83(403), 596-610. DOI: 10.1080/01621459.1988.10478639

[3] Cleveland, W. S., & Grosse, E. (1991). Computational methods for local regression. Statistics and computing, 1(1), 47-62. DOI: 10.1007/BF01890836

# API Reference

```@autodocs
Modules = [Lowess]
```