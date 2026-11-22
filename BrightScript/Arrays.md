# BrightScript Fundamentals 3: Arrays

This lesson continues from [Strings.md](Strings.md). An **array** holds an ordered collection of values in one variable. For example, an array can hold the titles of movies in a Roku app.

The examples below are short snippets. To run one, put it inside `sub Main()` and `end sub` in a `.brs` file, save it, and run it with `npx brs filename.brs`.

## 1. Create an array

Use square brackets and separate values with commas:

```brightscript
movies = ["Dune", "Coco", "Up"]
print Type(movies)    ' roArray
```

`roArray` is BrightScript's array object. You can also start with an empty array:

```brightscript
movies = []
```

Arrays can contain different types, such as `["Coco", 2024, true]`, although keeping related values consistent usually makes them easier to work with.

## 2. Access an item

Each item has a position called an **index**. Array indexes start at **0**:

```brightscript
movies = ["Dune", "Coco", "Up"]

print movies[0]    ' Dune: first item
print movies[1]    ' Coco: second item
print movies[2]    ' Up: third item
print movies[3]    ' invalid: no fourth item
```

The last index in an array of three items is `2`, not `3`. Reading an element that hasn't been set returns `invalid`, so check that the item exists before using it.

## 3. Change an item

Assign a new value to a particular index:

```brightscript
movies = ["Dune", "Coco", "Up"]
movies[1] = "Cars"

print movies[1]    ' Cars
```

The array is now `["Dune", "Cars", "Up"]`. The first and third items did not change.

Arrays are objects. Assigning an array to another variable does **not** create a separate copy; both variables refer to the same array:

```brightscript
movies = ["Dune", "Coco"]
otherMovies = movies
otherMovies[0] = "Soul"

print movies[0]    ' Soul
```

Changing the item through `otherMovies` also changes what you see through `movies` because they refer to the same array.

## 4. Count items

Use `.count()` to get the number of items:

```brightscript
movies = ["Dune", "Coco", "Up"]
print movies.count()    ' 3
```

You can use the count to check whether an array is empty before accessing its first item:

```brightscript
movies = ["Dune", "Coco", "Up"]

if movies.count() > 0 then
    print movies[0]    ' Dune
end if
```

For a non-empty array, the last index is `movies.count() - 1`.

## 5. Add an item

Use `.push()` to add an item at the end:

```brightscript
movies = ["Dune", "Coco", "Up"]
movies.push("Inside Out")

print movies.count()    ' 4
print movies[3]         ' Inside Out
```

Unlike a string method such as `.trim()`, `.push()` changes the existing array.

## 6. Remove items

Use `.pop()` to remove **and return** the last item:

```brightscript
movies = ["Dune", "Coco", "Up"]
removedMovie = movies.pop()

print removedMovie      ' Up
print movies.count()    ' 2
```

The array now contains `["Dune", "Coco"]`. Calling `.pop()` on an empty array returns `invalid`.

Use `.delete(index)` to remove an item at a specific position:

```brightscript
movies = ["Dune", "Coco", "Up"]
wasDeleted = movies.delete(1)

print wasDeleted        ' true
print movies.count()    ' 2
print movies[1]         ' Up
```

After `"Coco"` is removed, `"Up"` moves from index `2` to index `1`. Unlike `.pop()`, `.delete()` returns whether the removal succeeded, not the removed item.

## 7. Visit every item

Use `for each` when you need the values:

```brightscript
movies = ["Dune", "Coco", "Up"]

for each movie in movies
    print movie
end for
```

Output:

```text
Dune
Coco
Up
```

On each pass, `movie` receives the next item in the array. The loop ends after the last item.

If you also need each item's index, use a `for` loop. Check that the array isn't empty first:

```brightscript
movies = ["Dune", "Coco", "Up"]

if movies.count() > 0 then
    for i = 0 to movies.count() - 1
        print i; movies[i]
    end for
end if
```

Here `i` takes the values `0`, `1`, and `2`, and `movies[i]` reads the item at each index.

## References

- [Roku: `roArray`](https://developer.roku.com/docs/references/brightscript/components/roarray.md)
- [Roku: Array methods (`ifArray`)](https://developer.roku.com/docs/references/brightscript/interfaces/ifarray.md)
- [Roku: Program statements](https://developer.roku.com/dev/docs/program-statements)
