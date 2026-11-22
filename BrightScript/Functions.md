# BrightScript Fundamentals 5: Subs and Functions

This lesson continues from [AssociativeArrays.md](AssociativeArrays.md). A **sub** or **function** groups statements under a name so you can run them when you need them. This helps you avoid repeating code and makes a program easier to read.

Each example below is a complete program. To try one, put it in a `.brs` file, save it, and run `npx brs filename.brs` from your project folder.

## 1. `Main` is a sub

You've already used a sub: `Main`. The local interpreter starts by calling it.

```brightscript
sub Main()
    print "Hello!"
end sub
```

`sub Main()` starts the definition, and `end sub` marks its end. The parentheses are where inputs, called **parameters**, can go.

## 2. Call another sub

Define a sub outside `Main`, then call it by name:

```brightscript
sub Main()
    showWelcome()
    showWelcome()
end sub

sub showWelcome()
    print "Welcome to the app!"
end sub
```

Output:

```text
Welcome to the app!
Welcome to the app!
```

A definition tells BrightScript what `showWelcome` does. The statements inside it run when you **call** `showWelcome()`—in this example, twice. A sub is useful when you want to perform an action without returning a value.

## 3. Pass a value to a sub

A parameter lets you give a sub information to work with:

```brightscript
sub Main()
    showWelcome("Yazan")
    showWelcome("Maya")
end sub

sub showWelcome(name as string)
    print "Welcome, " + name + "!"
end sub
```

Output:

```text
Welcome, Yazan!
Welcome, Maya!
```

`name` is the parameter. `"Yazan"` is the **argument** passed in the first call. Inside the sub, `name` holds that value. `as string` specifies the type expected by the parameter.

## 4. Return a value from a function

A function can send a result back to its caller with `return`:

```brightscript
sub Main()
    total = add(4, 3)
    print total             ' 7
end sub

function add(a as integer, b as integer) as integer
    return a + b
end function
```

Here's the flow:

1. `Main` calls `add(4, 3)`.
2. Inside `add`, `a` is `4` and `b` is `3`.
3. `return a + b` sends `7` back.
4. `Main` stores that result in `total` and prints it.

The `as integer` after the closing parenthesis describes the function's return type. A function can also be used directly in an expression:

```brightscript
sub Main()
    print double(5) + 1     ' 11
end sub

function double(number as integer) as integer
    return number * 2
end function
```

## 5. Sub or function?

Use a **sub** to do something, such as printing a message. Use a **function** when the caller needs a result to store, print, or use in another calculation.

```brightscript
sub Main()
    message = makeGreeting("Yazan")
    showMessage(message)
end sub

function makeGreeting(name as string) as string
    return "Hello, " + name
end function

sub showMessage(text as string)
    print text             ' Hello, Yazan
end sub
```

`makeGreeting` returns the text; `showMessage` prints it. Returning a value and printing a value are different: `return` gives the result to the caller, while `print` displays it in the console.

## 6. Variables are local

A variable created inside a sub or function is local to that call. A different sub can have a variable with the same name without changing it:

```brightscript
sub Main()
    message = "Inside Main"
    showMessage()
    print message          ' Inside Main
end sub

sub showMessage()
    message = "Inside showMessage"
    print message          ' Inside showMessage
end sub
```

The output shows the sub's message first, followed by `Main`'s message. To share a value with a sub or function, pass it as an argument; to get a result back, use a function and `return`.

## 7. Optional parameters

You can give a parameter a default value. If the caller leaves it out, the default is used:

```brightscript
sub Main()
    print greet("Yazan")          ' Hello, Yazan
    print greet("Yazan", "Hi")    ' Hi, Yazan
end sub

function greet(name as string, greeting = "Hello" as string) as string
    return greeting + ", " + name
end function
```

Here, `greeting` defaults to `"Hello"`. When `"Hi"` is passed, it replaces the default for that call. Optional parameters come after required parameters.

## 8. Passing arrays and associative arrays

When you pass an array or an associative array, the caller and the sub refer to the same object. Changes to that object are visible to the caller:

```brightscript
sub Main()
    movies = ["Dune"]
    addMovie(movies, "Coco")

    print movies.count()    ' 2
    print movies[1]         ' Coco
end sub

sub addMovie(list as object, title as string)
    list.push(title)
end sub
```

`list` refers to the same array as `movies`, so `.push()` changes that shared array. The same idea applies to associative arrays. Simple values such as integers are passed by value: assigning a different integer to a parameter does not change the caller's variable.

## 9. Return early

`return` immediately ends a function. You can use it inside an `if` to handle a condition before continuing:

```brightscript
sub Main()
    print describeScore(5)     ' Keep practicing
    print describeScore(15)    ' Passed
end sub

function describeScore(score as integer) as string
    if score < 10 then
        return "Keep practicing"
    end if

    return "Passed"
end function
```

When `score` is below `10`, the first `return` ends the call, so the second one is not reached. Otherwise the function returns `"Passed"`.

## References

- [Roku: Program statements](https://developer.roku.com/dev/docs/program-statements)
- [Roku: Expressions, variables, and types](https://developer.roku.com/docs/references/brightscript/language/expressions-variables-types.md)
