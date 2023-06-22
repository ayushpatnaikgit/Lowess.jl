```@meta
CurrentModule = Lowess
```

Documentation for [Lowess](https://github.com/xKDR/Lowess.jl).

```@contents
Pages=["index.md"]
```

# Lowess

This package includes a pure Julia `lowess` function, which is an implementation
of the LOWESS smoother (references given at the end of the documentation), along with
a `lowess_model` function which can be used to make corresponding models.

# Tutorial

In this section we will go through a simple example to see how the package works. Consider the following code snippet.

```@example
using Lowess, Plots, Random
Random.seed!(42)

xs = 10 .* rand(100)
xs = sort(xs)
ys = sin.(xs) .+ 0.5 * rand(100)

model = lowess_model(xs, ys, 0.2)

us = range(extrema(xs)...; step = 0.1)
vs = model(us)

scatter(xs, ys)
plot!(us, vs, legend = false)
```

The above code creates some random data points sampled out of a sine curve, with some noise added to the ordinates. A model is created using this data with `f = 0.2` (`f` is the parameter which controls the amount of smoothing).

`us` is a vector of abscissas which lie in the range of the abscissas which were passed as input to the model. To get the predicted smooth values for `us`, we are passing it to the model; the result will be the vector `vs`, the vector of predicted values.

Finally, we get the scatter plot of the input points, and the smooth plot using `us` and `vs`.

# References

[1] Cleveland, W. S. (1979). Robust locally weighted regression and smoothing scatterplots. Journal of the American statistical association, 74(368), 829-836. DOI: 10.1080/01621459.1979.10481038

[2] Cleveland, W. S., & Devlin, S. J. (1988). Locally weighted regression: an approach to regression analysis by local fitting. Journal of the American statistical association, 83(403), 596-610. DOI: 10.1080/01621459.1988.10478639

[3] Cleveland, W. S., & Grosse, E. (1991). Computational methods for local regression. Statistics and computing, 1(1), 47-62. DOI: 10.1007/BF01890836

# API Reference

```@autodocs
Modules = [Lowess]
```
