I also have a [Futojulia](futojulia.md) section, for General Julia thingies!

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
* [https://github.com/jkrumbiegel/GridLayoutBase.jl/blob/2e692ab23adc7ad0878ac905a71053964dd15822/src/gridlayout.jl#L1530](https://github.com/jkrumbiegel/GridLayoutBase.jl/blob/2e692ab23adc7ad0878ac905a71053964dd15822/src/gridlayout.jl#L1530)

## 2. Exception stack
This one is not about Makie, it is about Julia. I don't care, imagine you're in the FutoJulia blog.

Let's say you had a try, and you caught it. 
```Julia
function boom()
  3 + "wtf"
end

try
  boom()
catch e
  @warn "caught!"
end
```
Great catch. What was the function call stack of the fail?

No idea.

The web [mostly](https://discourse.julialang.org/t/getting-a-stack-trace-with-function-argument-values/529/3) [says](https://discourse.julialang.org/t/inspecting-the-stack/376/5) catch_stacktrace(). But it is not working anymore.

Use stacktrace(catch_backtrace())):
```julia
try
  boom()
catch e
  @warn "caught!"
  stacktrace(catch_backtrace())
end
```
Aha! The issue is in boom().

### References
* The solution is actually found in the [Julia manual on stack traces](https://docs.julialang.org/en/v1/manual/stacktraces/#Error-handling). Just buried a bit.
* [https://stackoverflow.com/questions/72718578/julia-how-to-get-an-error-message-and-stacktrace-as-string](https://stackoverflow.com/questions/72718578/julia-how-to-get-an-error-message-and-stacktrace-as-string)
