# BrighterScript: Syntax Extensions

## Template strings

```brighterscript
name = "Asaad"
age = 30
print `${name} is ${age} years old`
```

No more `+` and `.toStr()` chains. Numbers convert automatically.

## Null-coalescing `??`

```brighterscript
title = movie.title ?? "Untitled"
```

Uses the right side only if the left is `invalid`.

## Optional chaining `?.`

```brighterscript
city = user?.address?.city
first = items?[0]
```

Returns `invalid` instead of crashing if something in the chain is `invalid`. Combine it with `??` for a default:

```brighterscript
city = user?.address?.city ?? "Unknown"
```

## Ternary

```brighterscript
label = isLive ? "LIVE" : "Recorded"
```

## Consts and enums

```brighterscript
const MAX_ITEMS = 20

enum Status
    loading
    ready
    failed
end enum

s = Status.ready
```

Both are compile-time only. They are replaced with their values in the output, so they cost nothing at runtime.

## Typed arrays

```brighterscript
function total(prices as float[]) as float
    result = 0.0
    for each p in prices
        result += p
    end for
    return result
end function
```

## Compound assignment

`+=`, `-=`, `*=`, `/=`, `++`, and `--` all work.