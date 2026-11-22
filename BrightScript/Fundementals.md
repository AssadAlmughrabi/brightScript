# BrightScript Fundamentals

BrightScript is the language that runs on Roku devices. It's dynamically typed, uses `sub`/`function` for procedures, and is built around Objects (arrays, associative arrays, and Roku components like `roDateTime`). Every Roku channel starts at `Main()`.

This guide pairs with `main.brs` in this repo. Every example was run with the local interpreter:

```bash
npx brs main.brs
```

> The local `brs` interpreter covers the core language but not every Roku component. Anything marked **Roku only** needs a real device (or the Roku emulator).

---

## 1. Hello, world

```brightscript
sub Main()
    print "Hello, World!"
end sub
```

- `Main` is the entry point. Roku looks for it and runs it first.
- Keywords are case-insensitive (`Sub`, `SUB`, `sub` all work), but the convention is lowercase.
- Statements are separated by newlines or colons (`a = 1 : b = 2`). There are no semicolons at line ends.

Run it:

```bash
npx brs main.brs
```

## 2. Comments

```brightscript
' A single-line comment
REM Also a single-line comment

' Multi-line:
' line one
' line two
```

## 3. Variables and types

BrightScript is dynamically typed: a variable's type comes from the value assigned to it, and can change later.

```brightscript
sub Main()
    n = 42              ' Integer
    pi = 3.14           ' Float
    label = "Hello"     ' String
    ready = true        ' Boolean
    nothing = invalid   ' Invalid (the "no value" value)
end sub
```

### The five primitive types

| Type | Example | Notes |
|---|---|---|
| Boolean | `true`, `false` | Result of comparisons |
| Integer | `42`, `-7` | 32-bit |
| LongInteger | `9876543210&` | 64-bit; use the `&` suffix or a large literal |
| Float | `3.14` | Single precision |
| Double | `3.141592653589793#` | Double precision; use the `#` suffix |
| String | `"Roku"` | Double quotes only |
| Invalid | `invalid` | Absence of a value |

### Type declaration suffixes

A suffix on the name forces a type. `a`, `a$`, and `a%` are three different variables.

```brightscript
count% = 7 / 2      ' Integer suffix -> 3 (truncated)
price! = 2.5        ' Float
name$ = "Roku"      ' String
big& = 9876543210   ' LongInteger
ratio# = 1 / 3      ' Double
```

### Checking types at runtime

`Type()` returns a string:

```brightscript
print Type(1)        ' Integer
print Type(1.5)      ' Float
print Type("s")      ' String
print Type(true)     ' Boolean
print Type(invalid)  ' Invalid
print Type([1, 2])   ' roArray
```

## 4. The `print` statement

`print` writes to the debug console (on a Roku, the telnet console). It accepts several values per line:

```brightscript
print "Hello"              ' Hello
print "Total:"; 5 + 3      ' Total:   8   (; separates with a tab)
print 1; 2; 3              ' 1   2   3
```

A trailing `;` suppresses the newline:

```brightscript
print "Loading";
print "..."
' Loading...
```

## 5. Operators

### Arithmetic

```brightscript
print 7 + 2     ' 9
print 7 - 2     ' 5
print 7 * 2     ' 14
print 7 / 2     ' 3.5   <- division produces a Float
print 7 \ 2     ' 3     <- integer division
print 7 MOD 2   ' 1
print 2 ^ 10    ' 1024
```

`/` always returns a Float. Use `\` for integer division. To keep an integer result, assign to a `%` variable or wrap with `Int()`.

### Comparison

```brightscript
print 1 = 1     ' true    (= is assignment AND equality, context decides)
print 1 <> 2    ' true    (<> means "not equal")
print 2 > 1     ' true
print 1 >= 1    ' true
```

String comparison is case-sensitive and compares character codes:

```brightscript
print "abc" = "ABC"   ' false
print "a" < "B"       ' false
```

### Logical

```brightscript
print not true            ' false
print true and false      ' false
print false or true       ' true
```

> There is no `!`, `&&`, or `||`. Use `not`, `and`, `or`.

## 6. Control flow

### if / else if / else

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

Single-line form:

```brightscript
if n = 3 then print "three"
if n = 4 then print "four" else print "not four"
```

### for / next

```brightscript
for i = 1 to 3
    print i           ' 1 2 3
end for

for i = 10 to 0 step -5
    print i           ' 10 5 0
end for

for i = 1 to 10
    if i = 3 then exit for
    print i           ' 1 2
end for
```

### for each

`for each` walks any Object with an enumerator: arrays, associative arrays, strings, etc.

```brightscript
for each value in [1, 2, 3]
    print value       ' 1 2 3
end for

cfg = { width: 1920, height: 1080 }
for each key in cfg
    print key         ' height / width (AA iteration yields KEYS, in no guaranteed order)
end for
```

### while

```brightscript
n = 0
while n < 3
    n = n + 1
    print n
end while

' "while true" plus exit while is the usual "loop until" pattern
i = 0
while true
    i = i + 1
    if i = 3 then exit while
end while
print i               ' 3
```

> There is no `break`, `continue`, or `++`. Use `exit for` / `exit while`, and `i = i + 1`.

## 7. Subs and functions

- `sub` performs an action and returns nothing.
- `function` returns a value with `return`.

```brightscript
sub Main()
    print add(2, 3)         ' 5
    print greet("Yazan")    ' Hello, Yazan
    show()
end sub

function add(a as integer, b as integer) as integer
    return a + b
end function

function greet(name as string, word = "Hello" as string) as string
    return word + ", " + name
end function

sub show()
    print "from a sub"
end sub
```

- Parameter and return types are optional but useful documentation; mismatches throw runtime errors.
- Default values: `word = "Hello" as string`.
- Unlike some languages, a bare expression is not a statement, so `a + b` on its own is a syntax error. Always write `return a + b`.
- Procedures can be defined in any order; the file is parsed before anything runs.

### Passing values around

Primitives are copied; Objects (arrays, AAs, components) are passed by reference, so changes inside the procedure are visible outside:

```brightscript
sub Main()
    counter = { count: 0 }
    bump(counter)
    print counter.count     ' 1

    n = 1
    setIt(n)
    print n                 ' 1 (integers are copied)
end sub

sub bump(obj as object)
    obj.count = obj.count + 1
end sub

sub setIt(v as integer)
    v = 99
end sub
```

### `m`

Inside a procedure, `m` refers to the enclosing scope. In a plain `.brs` script it's the global associative array (`GetGlobalAA()`); inside a SceneGraph component it's the component's own fields. You'll use `m.top`, `m.global`, and per-component state constantly in Roku work.

## 8. Arrays

Arrays (`roArray`) are ordered, zero-indexed, and grow dynamically.

```brightscript
sub Main()
    a = [3, 1, 2]
    print a[0]          ' 3
    a[0] = 7
    print a[0]          ' 7
    print a.count()     ' 3

    a.push(9)           ' append
    print a.count()     ' 4
    print a.pop()       ' 9 (removes and returns last)
    print a.count()     ' 3
end sub
```

Useful methods: `count()`, `push()`, `pop()`, `shift()`, `unshift()`, `delete(index)`, `join(separator)`.

`join` only works when every element is a String:

```brightscript
print ["a", "b"].join("-")    ' a-b
```

Array literals can mix types, but that usually causes trouble later:

```brightscript
mixed = [1, "two", true]      ' allowed, but operations like join() will fail
```

## 9. Associative arrays

An associative array (`roAssociativeArray`, often "AA") is BrightScript's hash map / record. It's the workhorse of Roku development.

```brightscript
sub Main()
    person = { name: "Ada", age: 36 }

    print person.name           ' Ada     (dot access)
    print person["age"]         ' 36      (bracket access; keys can be dynamic)
    person.city = "London"      ' adding a key
    print person.city           ' London

    print person.missing        ' invalid (no error)
    print person.lookup("nope") ' invalid
    print person.doesExist("age")    ' true
    print person.doesExist("depth")  ' false

    person.addReplace("age", 37)     ' update or insert
    person.delete("city")
    print person.count()             ' 2
    print person.keys()              ' roArray of key names
end sub
```

Notes:

- Keys are case-insensitive: `person.Name` and `person.name` are the same key.
- Reading a missing key returns `invalid`; it does not throw.
- AAs are the natural way to model config and data:

```brightscript
show = {
    title: "My Show",
    seasons: 3,
    episodes: [ { number: 1, name: "Pilot" }, { number: 2, name: "Second" } ]
}
print show.episodes[1].name     ' Second
```

## 10. Converting between types

```brightscript
print Str(42)        ' " 42"  (note the leading space for positives)
print Val("3.5") + 1 ' 4.5
print Int(3.9)       ' 3     (floor)
print Fix(3.9)       ' 3     (truncate toward zero)
print Int(-3.9)      ' -4
print Fix(-3.9)      ' -3
```

`Str()` on a positive number includes a leading space, so build strings with `;` in `print` or concatenate only strings:

```brightscript
print "Total: "; 5 + 3     ' Total:   8   (fine)
print "n = " + Str(5)      ' n =  5      (extra space from Str)
```

Adding a String and a number is an error:

```brightscript
' print "n = " + 5        ' Runtime error: non-homogeneous values
```

## 11. Common beginner gotchas

1. **A compile error stops everything.** Nothing runs until the whole file parses. If your program prints nothing, run it and read the first error line — don't debug the logic yet.
2. **`=` is both assignment and comparison.** `if x = 5 then` compares; `x = 5` assigns.
3. **No `++`, `--`, `break`, `continue`, `+=`.** Write `i = i + 1`, `exit for`, `exit while`.
4. **String + number is an error.** Use `;` inside `print`, or convert explicitly.
5. **Suffixed names are separate variables.** `count` and `count%` are not the same thing, and reading the unsuffixed one can yield `<uninitialized>`.
6. **Only `Main` runs automatically.** Other subs/functions run only when called, or when they're event handlers registered by a component (`init`, `onKeyEvent`, etc.).
7. **Reserved words can't be identifiers.** e.g. `box`, `type`, `print`, `end`, `next`, `mod`.
8. **Missing keys return `invalid`, not an error.** Check with `doesExist()` or compare to `invalid` before using a value.
9. **Strings are case-sensitive** for `=` and ordering, and there's no `ToLower` method on the base string in old Roku OS versions — use the global `LCase()`/`UCase()`.

## 12. Where this fits on Roku

A plain `.brs` file is enough for the local interpreter, but a Roku channel is a folder shaped like this:

```
my-channel/
├── manifest
├── source/
│   └── main.brs          <- sub Main() starts the app
├── components/
│   ├── MainScene.xml     <- SceneGraph views (UI)
│   └── MainScene.brs     <- SceneGraph logic (init, onKeyEvent, observers)
└── images/
```

- **SceneGraph** (XML + `.brs`) builds the TV UI: `Label`, `RowList`, `Video`, etc.
- `source/main.brs` creates the scene: `m.top.AppendChild(CreateObject("roSGNode", "MainScene"))`.
- **Roku only:** `CreateObject("roDeviceInfo")`, `roDateTime`, `roUrlTransfer`, and all scene nodes need a device or emulator; the local `brs` interpreter won't run them.

## Quick reference

```brightscript
' Types:    Boolean, Integer, LongInteger, Float, Double, String, Invalid, Objects
' Suffixes: % & ! # $
' Print:    print a; b; c
' Divide:   / (float)   \ (integer)   MOD
' Compare:  = <> > >= < <=
' Logic:    and or not
' Loops:    for i = a to b step s ... end for
'           for each x in obj ... end for
'           while cond ... end while    exit for / exit while
' Branch:   if c then ... else if c then ... else ... end if
'           if c then stmt          (single line)
' Functions: function f(a as integer) as integer / return x / end function
' Subs:      sub s(a as string) / end sub
' Arrays:    a = [1, 2]; a.push(3); a[0]; a.count()
' AAs:       o = { k: 1 }; o.k; o["k"]; o.lookup("k"); o.doesExist("k")
' Entry:     sub Main()
```
