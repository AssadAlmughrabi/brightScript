# BrightScript Fundamentals 7: Loops

This lesson continues from [Conditions.md](Conditions.md). A **loop** repeats a block of code. Loops are useful when you want to count, visit every movie in a list, or keep working while a condition is true.

Each BrightScript example is a complete program. Put one in a `.brs` file, save it, and run `npx brs filename.brs` from your project folder.

## 1. Count with `for`

```brightscript
sub Main()
    for i = 1 to 3
        print i
    end for
end sub
```

Output:

```text
1
2
3
```

The variable `i` starts at `1`, increases by `1` after each pass, and reaches `3`. Both ends of this range are included. The code between `for` and `end for` is called the **loop body**.

## 2. Change the step

Use `step` to choose how much the counter changes each time:

```brightscript
sub Main()
    for i = 10 to 0 step -5
        print i
    end for
end sub
```

Output:

```text
10
5
0
```

A negative step counts down. Here the counter starts at `10`, subtracts `5`, and stops after reaching `0`.

## 3. Visit array items with `for each`

```brightscript
sub Main()
    movies = ["Dune", "Coco", "Up"]

    for each movie in movies
        print movie
    end for
end sub
```

Output:

```text
Dune
Coco
Up
```

On each pass, `movie` receives the next value in the array. Use `for each` when you want the values and don't need their positions. For an associative array, `for each` supplies the **keys** instead; see [AssociativeArrays.md](AssociativeArrays.md).

## 4. Loop with an index

If you need the position as well as the value, use an indexed `for` loop:

```brightscript
sub Main()
    movies = ["Dune", "Coco", "Up"]

    if movies.count() > 0 then
        for i = 0 to movies.count() - 1
            print i; movies[i]
        end for
    end if
end sub
```

The printed positions are `0`, `1`, and `2`. Arrays start at zero, so the last valid index here is `count() - 1`. The `if` keeps this example from trying to loop over an empty array.

## 5. Repeat with `while`

A `while` loop checks its condition before each pass:

```brightscript
sub Main()
    remaining = 3

    while remaining > 0
        print remaining
        remaining = remaining - 1
    end while

    print "Done"
end sub
```

Output:

```text
3
2
1
Done
```

When `remaining` reaches `0`, the condition becomes false and the loop finishes. Make sure something can eventually make the condition false, or a loop can run forever.

## 6. Stop a loop early

Use `exit for` to stop the current `for` or `for each` loop:

```brightscript
sub Main()
    movies = ["Dune", "Coco", "Up"]

    for each movie in movies
        if movie = "Coco" then
            print "Found Coco"
            exit for
        end if
    end for

    print "Search finished"
end sub
```

The program prints `Found Coco` and then `Search finished`; it does not inspect `Up`. Use `exit while` to leave a `while` loop early.

## References

- [Roku: Program statements](https://developer.roku.com/dev/docs/program-statements)
- [Roku: `roArray`](https://developer.roku.com/docs/references/brightscript/components/roarray.md)
