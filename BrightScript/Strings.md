# BrightScript Fundamentals 2: Strings and Text

This lesson continues from [Fundementals.md](Fundementals.md). You'll learn how to create, combine, and work with text in BrightScript.

A **string** is a value containing text, such as `"Hello"` or `"Roku TV"`.

## 1. Create a string

```brightscript
name = "Yazan"
platform = "Roku"
```

Text goes inside double quotes. Without quotes, BrightScript treats the word as a variable name instead of text.

```brightscript
print name       ' Prints: Yazan
print "name"     ' Prints: name
```

> The short examples in sections 1–7 belong inside `sub Main()` when you run them.

## 2. Combine strings

Use `+` to join strings. This is called **concatenation**.

```brightscript
name = "Yazan"
greeting = "Hello, " + name + "!"
print greeting
```

Output:

```text
Hello, Yazan!
```

Notice the space inside `"Hello, "`: without it, the result would be `Hello,Yazan!`.

## 3. Put quotation marks inside text

Write two double quotes (`""`) when you want one quotation mark inside a string:

```brightscript
print "She said ""Hello!"""
```

Output:

```text
She said "Hello!"
```

A single quote (`'`) starts a comment when it appears outside a string. It can be used normally inside double quotes, as in `"Yazan's TV"`.

## 4. Find the length

Use `.len()` to count the characters in a string. Spaces and punctuation count too.

```brightscript
title = "BrightScript"
print title.len()     ' 12
```

The parentheses call the `len` method. A **method** is an action you call on a value using a dot, such as `title.len()`.

## 5. Take part of a string

```brightscript
title = "BrightScript"
print title.left(6)      ' Bright
print title.right(6)     ' Script
print title.mid(6, 6)    ' Script
```

- `left(6)` takes the first six characters.
- `right(6)` takes the last six characters.
- `mid(6, 6)` starts at position 6 and takes six characters.

String **method** positions start at **0**, not 1. In `"BrightScript"`, `B` is at position `0`, and `S` is at position `6`. Other functions can use different indexing rules, so check the function you're using.

These methods return new strings; they do not change `title` unless you assign the result back to a variable.

## 6. Search and change letter case

```brightscript
title = "BrightScript"
print title.instr("Script")   ' 6
print title.instr("Roku")     ' -1: not found

print UCase("Roku")           ' ROKU
print LCase("Roku")           ' roku
```

`.instr()` returns the position where the text starts, or `-1` if it isn't found. Use `>= 0` to check whether it was found:

```brightscript
if title.instr("Script") >= 0 then
    print "Found it!"
end if
```

`UCase()` and `LCase()` are functions: you pass the text inside their parentheses. They return a result rather than changing the original variable.

## 7. Trim, split, and numbers

### Remove extra spaces

`.trim()` removes whitespace from the beginning and end, not the middle:

```brightscript
text = "  Roku TV  "
print text.trim()     ' Roku TV
```

### Split text into an array

`.split()` returns an array of strings:

```brightscript
genres = "Action,Comedy,Drama".split(",")
print genres.count()  ' 3
print genres[0]       ' Action
print genres[1]       ' Comedy
```

Arrays also start at index `0`, so `genres[0]` is the first item.

### Show numbers with text

You cannot add a string and a number directly with `+`. Convert the number first:

```brightscript
score = 15
print "Score: " + Str(score).trim()   ' Score: 15
```

`Str()` converts the number to text. For a positive number it adds a leading space, so `.trim()` removes that space here. You can also print values separately with `print "Score:"; score`.

## References

- [Roku: String operations (`ifStringOps`)](https://developer.roku.com/docs/references/brightscript/interfaces/ifstringops.md)
- [Roku: Expressions, variables, and types](https://developer.roku.com/docs/references/brightscript/language/expressions-variables-types.md)
