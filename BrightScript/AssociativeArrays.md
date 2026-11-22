# BrightScript Fundamentals 4: Associative Arrays

This lesson continues from [Arrays.md](Arrays.md). An **associative array** stores values under names you choose. Each name is called a **key**, and the value stored under it is the **value**.

An array uses positions (`movies[0]`, `movies[1]`). An associative array uses names (`movie.title`, `movie.year`). This makes it a natural fit for describing things like a movie, a channel, or a login result.

The examples below are short snippets. To run one, put it inside `sub Main()` and `end sub` in a `.brs` file, save it, and run it with `npx brs filename.brs`.

## 1. Create an associative array

Use curly braces. Write each key, a colon, and its value:

```brightscript
movie = {
    title: "Dune",
    year: 2021,
    rating: 8.1
}

print Type(movie)    ' roAssociativeArray
```

`roAssociativeArray` is BrightScript's name for this object. You can also start with an empty one:

```brightscript
movie = {}
```

## 2. Access a value

Use a dot and the key name. You can also use square brackets, which lets you look up a key stored in a variable:

```brightscript
movie = { title: "Dune", year: 2021 }

print movie.title          ' Dune
print movie["year"]        ' 2021

fieldName = "year"
print movie[fieldName]     ' 2021
```

The bracket form is useful when the key comes from somewhere else, such as a function result or a loop variable.

Keys are **case-insensitive**. These read the same value:

```brightscript
movie = { title: "Dune" }

print movie.title    ' Dune
print movie.Title    ' Dune
```

## 3. Missing keys

Reading a key that doesn't exist does not stop the program. It returns `invalid`:

```brightscript
movie = { title: "Dune" }

print movie.director         ' invalid
print movie.lookup("year")   ' invalid
```

Before using a value, check that its key exists with `.doesExist()`:

```brightscript
movie = { title: "Dune", year: 2021 }

if movie.doesExist("year") then
    print movie.year         ' 2021
else
    print "Year unknown"
end if
```

Without the check, you might pass `invalid` into later code and get a more confusing error.

## 4. Change and add values

Assign to an existing key to change it, or to a new key to add it:

```brightscript
movie = { title: "Dune", year: 2021 }

movie.year = 2020                 ' change
movie.director = "Denis Villeneuve"   ' add

print movie.year        ' 2020
print movie.director    ' Denis Villeneuve
```

`.addReplace(key, value)` does both operations in one call: it replaces the value if the key exists, or adds it if it doesn't:

```brightscript
movie = { title: "Dune", year: 2021 }
movie.addReplace("year", 2019)

print movie.year        ' 2019
```

Like arrays, associative arrays are objects. Assigning one to another variable does not create a copy:

```brightscript
first = { title: "Dune" }
second = first
second.title = "Soul"

print first.title       ' Soul
```

Both variables refer to the same object, so changing one is visible through the other.

## 5. Count and list keys

`.count()` returns how many keys the object has. `.keys()` returns an array of the key names:

```brightscript
movie = { title: "Dune", year: 2021, rating: 8.1 }

print movie.count()     ' 3
print movie.keys()      ' roArray: title, year, rating
```

`.delete(key)` removes an entry and reports whether it worked:

```brightscript
movie = { title: "Dune", year: 2021 }

removed = movie.delete("year")
print removed           ' true
print movie.count()     ' 1

removed = movie.delete("nope")
print removed           ' false
```

## 6. Store arrays and other objects

Values can be any type, including arrays and other associative arrays:

```brightscript
film = {
    title: "Dune",
    cast: ["Chalamet", "Zendaya"],
    video: { width: 1920, height: 1080 }
}

print film.cast[0]          ' Chalamet
print film.cast.count()     ' 2
print film.video.width      ' 1920
```

Read it from the outside in: `film.video.width` means "the `width` value inside the `video` object inside `film`." This is how Roku apps usually describe settings, like screen size or playback position.

## 7. Visit every entry

`for each` over an associative array gives you the **keys**, not the values:

```brightscript
movie = { title: "Dune", year: 2021 }

for each key in movie
    print key; movie[key]
end for
```

Output (order may vary):

```text
title Dune
year 2021
```

Use the key with `movie[key]` to get its value. Do not rely on the order of entries; use a specific key when order matters.

## References

- [Roku: `roAssociativeArray`](https://developer.roku.com/docs/references/brightscript/components/roassociativearray.md)
- [Roku: Associative array methods (`ifAssociativeArray`)](https://developer.roku.com/docs/references/brightscript/interfaces/ifassociativearray.md)
