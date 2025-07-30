## 1. Functional layout populating
Let's say you have two sub-grids within a parent grid:
```julia
fig = Figure()

foo = GridLayout()
fig[1, 2] = foo

bar = GridLayout()
foo[1, 1] = bar
baz = GridLayout()
foo[1, 2] = baz

Label(bar, "bar")
```

Normally if we want to move the content of bar to be where baz is located? we'll just move bar into baz's location:
```julia
foo[1, 2] = bar
```
(technically this is putting a Makie.GridLayoutBase.GridLayout into a Makie.GridLayoutBase.GridPosition.)

But if we want this to be functional?
setindex!() is the solution:
```julia
setindex!(foo, bar, 1, 1)
```

### References
* https://github.com/jkrumbiegel/GridLayoutBase.jl/blob/2e692ab23adc7ad0878ac905a71053964dd15822/src/gridlayout.jl#L1530
