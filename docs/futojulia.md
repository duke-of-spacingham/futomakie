# Thingies I realized about Julia

## 2) Referencing an array
There are no arrays. There's a vector. Now...

So you want to create a reference to a vector
```julia
a = [1, 2, 3]
b = Ref(a)
a[1] = 42
println(b[])
```
```bash
[42, 2, 3]
```
yay! works.

Now change the whole vector:
```julia
a = [1, 2, 3]
b = Ref(a)
a = [42, 43, 44]
println(b[])
```
```bash
[1, 2, 3]
```
not yay.

What happened?

well, first of all the Ref() is redundant. A vector is a reference by itself. So let's work with this, it does the exact same thing:
```julia
a = [1, 2, 3]
b = a
a = [42, 43, 44]
println(b)
```

Now, two points here:
1) b = a make "b" another name of the vector to which "a" refers. That's right, it has nothing to do with "a" itself, it just shares the same value with it.
2) Unlike changing a value within the "a" vector, Reassigning a vector to "a" means making a refer to another vector. making a and b refer to two different vectors.

So you changed the vector "a" refers to. "b" doesn't care, it still refers the first vector.

But we want "b" to care. We want "b" to change together with "a" even when we're changing the whole array. sharing is caring and all this crap.

The solution: Don't change a's referenced vector. change the contents of a's vector:

```julia
a = [1, 2, 3]
b = a
a[:] = [4, 5, 6]
println(b)
```
```bash
[4, 5, 6]
```
yay.

### References
* [https://www.johnmyleswhite.com/notebook/2014/09/06/values-vs-bindings-the-map-is-not-the-territory/](https://www.johnmyleswhite.com/notebook/2014/09/06/values-vs-bindings-the-map-is-not-the-territory/)
