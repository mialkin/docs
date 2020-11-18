# Tricks

## `ref` vs `out`

`ref` tells the compiler that the object is initialized before entering the function, while `out` tells the compiler that the object will be initialized inside the function.

The `ref` modifier means that:

- The value is already set
- The method can read and modify it

The `out` modifier means that:

- The value isn't set and can't be read by the method *until* it is set
- The method *must* set it before returning
