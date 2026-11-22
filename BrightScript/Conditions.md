# BrightScript Fundamentals 6: Conditions

This lesson continues from [Functions.md](Functions.md). A **condition** is an expression that produces `true` or `false`. Use conditions to decide which code runs.

Each BrightScript example is a complete program. Put one in a `.brs` file, save it, and run `npx brs filename.brs` from your project folder.

## 1. Make a decision with `if`

```brightscript
sub Main()
    score = 15

    if score >= 10 then
        print "Passed"
    end if
end sub
```

`score >= 10` asks whether the score is greater than or equal to `10`. It is true, so the program prints `Passed`. If the condition is false, the statements inside the `if` block are skipped.

## 2. Choose between two paths

```brightscript
sub Main()
    score = 5

    if score >= 10 then
        print "Passed"
    else
        print "Keep practicing"
    end if
end sub
```

The score is below `10`, so the output is `Keep practicing`. Only one of the two blocks runs.

## 3. Check multiple conditions

Use `else if` for another possible case:

```brightscript
sub Main()
    score = 75

    if score >= 90 then
        print "A"
    else if score >= 70 then
        print "B"
    else
        print "C"
    end if
end sub
```

The output is `B`. Conditions are checked from top to bottom; once one matches, the remaining branches are skipped. Order matters: if `score >= 70` came first, a score of `95` would also match that branch.

## 4. Compare values

| Operator | Meaning | Example |
|---|---|---|
| `=` | Equal to | `score = 10` |
| `<>` | Not equal to | `score <> 10` |
| `>` | Greater than | `score > 10` |
| `>=` | Greater than or equal to | `score >= 10` |
| `<` | Less than | `score < 10` |
| `<=` | Less than or equal to | `score <= 10` |

```brightscript
sub Main()
    name = "Roku"

    print name = "Roku"    ' true
    print name = "roku"    ' false
    print 5 <> 10          ' true
end sub
```

String value comparisons are case-sensitive. Note that `=` is also used for assignment: `name = "Roku"` sets a variable, but `if name = "Roku" then` compares it.

## 5. Combine conditions

Use `and` when **both** conditions must be true, `or` when **at least one** must be true, and `not` to reverse a Boolean value:

```brightscript
sub Main()
    signedIn = true
    hasSubscription = false

    print signedIn and hasSubscription    ' false
    print signedIn or hasSubscription     ' true
    print not signedIn                     ' false
end sub
```

Use parentheses to make longer expressions easier to read:

```brightscript
sub Main()
    age = 12
    rating = "PG"

    if (age < 13) and (rating = "PG") then
        print "Show the family category"
    end if
end sub
```

## 6. Check `invalid` explicitly

`invalid` means there is no usable value. Compare it explicitly before using the value:

```brightscript
sub Main()
    movie = { title: "Dune" }
    year = movie.year

    if year = invalid then
        print "Year is missing"
    else
        print year
    end if
end sub
```

The key `year` is missing, so this prints `Year is missing`. [TypesAndInvalid.md](TypesAndInvalid.md) explains this in more detail.

## References

- [Roku: Program statements](https://developer.roku.com/dev/docs/program-statements)
- [Roku: Expressions, variables, and types](https://developer.roku.com/docs/references/brightscript/language/expressions-variables-types.md)
