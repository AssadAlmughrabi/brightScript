# BrightScript Fundamentals 8: Types and `invalid`

This lesson continues from [Loops.md](Loops.md). A **type** describes the kind of value you're working with, such as a number or a string. `invalid` is a special value used when there is no usable value.

Each BrightScript example is a complete program. Put one in a `.brs` file, save it, and run `npx brs filename.brs` from your project folder.

## 1. Common types

```brightscript
sub Main()
    title = "Dune"       ' String: text
    year = 2021          ' Integer: whole number
    rating = 8.5         ' Float: decimal number
    isAvailable = true   ' Boolean: true or false
    missing = invalid    ' Invalid: no usable value
    movies = ["Dune"]    ' roArray: list of values
    movie = { title: "Dune" }  ' roAssociativeArray: named values
end sub
```

BrightScript has other types, including `LongInteger` and `Double`. Arrays and associative arrays are objects, as described in the earlier lessons.

## 2. Check a value's type

`Type()` returns the type name as a string:

```brightscript
sub Main()
    print Type("Dune")       ' String
    print Type(2021)         ' Integer
    print Type(8.5)          ' Float
    print Type(true)         ' Boolean
    print Type(invalid)      ' Invalid
    print Type(["Dune"])     ' roArray
end sub
```

This is useful when debugging a value that isn't behaving as expected.

## 3. Variables can change type

An ordinary variable takes the type of the value assigned to it:

```brightscript
sub Main()
    value = 10
    print Type(value)    ' Integer

    value = "ten"
    print Type(value)    ' String
end sub
```

This is called **dynamic typing**. You don't need a separate declaration to create an ordinary variable. Type-suffixed names such as `count%` are a separate feature; `count` and `count%` are different variables.

## 4. Understand `invalid`

A missing associative-array key returns `invalid`:

```brightscript
sub Main()
    movie = { title: "Dune" }
    year = movie.year

    if year = invalid then
        print "Year is unavailable"
    end if
end sub
```

Don't try to call a method or access a property on `invalid`. Check it first, then use it only when it contains a suitable value.

## 5. Missing key versus `invalid` value

`.doesExist()` checks whether a key is present, even if its value is `invalid`:

```brightscript
sub Main()
    movie = { title: "Dune", year: invalid }

    print movie.doesExist("year")    ' true: the key exists
    print movie.year = invalid       ' true: its value is invalid
    print movie.doesExist("rating")  ' false: no such key
end sub
```

Choose the check that answers your question: does the key exist, or does it contain a usable value? A present key can still contain `invalid`.

## 6. Convert between text and numbers

A string and a number cannot be joined directly with `+`. `Str()` turns a number into text, and `Val()` reads a number from text:

```brightscript
sub Main()
    score = 15
    print "Score: " + Str(score).trim()    ' Score: 15

    price = Val("3.5")
    print price + 1                        ' 4.5
end sub
```

For positive numbers, `Str()` adds a leading space; `.trim()` removes it here. Use `Val()` with input you know contains a number: converting text is not the same as validating that input.

## 7. Check before calling a method

If input could be missing or of the wrong type, guard the operation:

```brightscript
sub Main()
    movie = { title: "Dune" }
    title = movie.title

    if Type(title) = "String" then
        print title.len()    ' 4
    else
        print "Title is missing or not text"
    end if
end sub
```

This check protects `.len()` in this example. In a larger app you may also encounter boxed strings, so decide which types your code accepts rather than assuming every source has the same representation.

## References

- [Roku: Expressions, variables, and types](https://developer.roku.com/docs/references/brightscript/language/expressions-variables-types.md)
- [Roku: Runtime functions](https://developer.roku.com/dev/docs/runtime-functions)
- [Roku: Global string functions](https://developer.roku.com/dev/docs/global-string-functions)
