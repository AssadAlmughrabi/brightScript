# BrightScript Fundamentals 9: Debugging

This lesson continues from [TypesAndInvalid.md](TypesAndInvalid.md). **Debugging** means finding out why code behaves differently from what you expected, then correcting the cause.

The commands below use the local `brs` interpreter from your `bright-script` folder. The BrightScript examples are complete programs; the ones marked **intentionally broken** show errors on purpose.

## 1. Start with the file you're actually running

Save your changes with **Cmd+S** in VS Code, then run the filename you edited:

```bash
npx brs main.brs
```

If the output looks old, check the tab for an unsaved-change dot, save it, and run the command again. `pwd` shows the terminal's curren t folder; `ls` lists its files. A filename in the command is relative to that folder unless you provide a path.

The BrightScript extension helps edit the code; saving the file and running the program are separate actions.

## 2. Syntax errors

A syntax error means the interpreter cannot understand the program's structure. This example is **intentionally broken**: the `if` is missing `end if`.

```brightscript
sub Main()
    print "Started"
    if true then
        print "Hello"
end sub
```

The local interpreter reports an unexpected `end sub` and says it expected an `end if`, `else if`, or `else`. It reports a parsing error and does not run this program, so `Started` does not print.

The location in an error message points to where the interpreter noticed a problem. The actual mistake can be earlier: here, `end if` is missing before `end sub`.

```brightscript
sub Main()
    print "Started"
    if true then
        print "Hello"
    end if
end sub
```

This corrected program prints `Started`, then `Hello`.

## 3. Runtime errors

A runtime error happens after the program starts. This example is **intentionally broken**:

```brightscript
sub Main()
    movie = { title: "Dune" }
    print "Before"
    print movie.year.len()
    print "After"
end sub
```

The local interpreter prints `Before`, then reports that `len` cannot be called. `movie.year` is `invalid` because there is no `year` key. It doesn't reach `After`.

Check the value before using it:

```brightscript
sub Main()
    movie = { title: "Dune" }
    year = movie.year

    if year = invalid then
        print "Year is unavailable"
    else
        print year
    end if
end sub
```

This prints `Year is unavailable` instead of trying to use the missing value.

## 4. Trace what the program sees

Print values at useful points to check your assumptions:

```brightscript
sub Main()
    score = 5
    print "Before:"; score

    score = score + 10
    print "After:"; score
    print "Type:"; Type(score)
end sub
```

The output shows the score before and after the change (`5` and `15`), followed by its type (`Integer`). Labels make it easier to tell which line produced each value. Remove or reduce temporary debugging prints when you no longer need them; avoid printing passwords, tokens, or other private data.

## 5. Read errors one step at a time

1. Save the file and run the exact filename you edited.
2. Read the first error message and its file and line number. Check nearby lines too.
3. Identify the value involved. A labeled `print` and `Type()` can help.
4. Change one thing, save, and run again. Confirm the output matches your expectation.

For `brs@0.45.0`, a runtime error can be printed even when the command returns exit status `0`, so don't rely on the exit status alone when using that interpreter. Check its output for errors.

## 6. Local interpreter versus Roku

`npx brs` is useful for trying core BrightScript. It is not a complete Roku device: a program that works locally may still need testing on Roku OS when it uses SceneGraph or device components. On a Roku device, use the BrightScript console and debugger described in Roku's documentation to inspect app output and failures.

## References

- [Roku: Debugging](https://developer.roku.com/dev/docs/debugging)
- [Roku: Runtime functions (`Type`)](https://developer.roku.com/dev/docs/runtime-functions)
