# BrighterScript 1: Introduction

You already know BrightScript. BrighterScript builds on it: you can use familiar BrightScript syntax and add features such as classes, namespaces, and imports.

BrighterScript is similar to TypeScript in one important way: you write code in a richer language, then a compiler converts it to the language the platform runs. For Roku, the output is BrightScript.

```text
BrighterScript (.bs) → BrighterScript compiler → BrightScript (.brs) → Roku
```

The compiler converts the source before the app is deployed. Roku does not run `.bs` files directly.

## 1. What stays the same?

Variables, conditions, loops, functions, arrays, and associative arrays still work in BrighterScript:

```brightscript
sub Main()
    movies = ["Dune", "Coco"]

    for each movie in movies
        print movie
    end for
end sub
```

This is familiar BrightScript syntax, and it is also valid inside a BrighterScript `.bs` file. Your existing BrightScript knowledge is the foundation for what comes next.

## 2. What does BrighterScript add?

BrighterScript adds ways to organize larger programs. For example:

- **Classes** group data and behavior together.
- **Namespaces** organize names into groups.
- **Imports** let you reference code from other files.

These are BrighterScript source features. The compiler transforms them into BrightScript before the code is packaged for Roku.

## 3. Your first class

A **class** describes how to create an object. The object can hold data and provide functions that use that data.

```brightscript
sub Main()
    selectedMovie = new Movie("Dune")
    print selectedMovie.getTitle()
end sub

class Movie
    title as string

    sub new(title as string)
        m.title = title
    end sub

    function getTitle() as string
        return m.title
    end function
end class
```

After compilation, this program prints:

```text
Dune
```

Here is what happens:

1. `class Movie` defines the shape and behavior of a movie object.
2. `title as string` declares a field for its title.
3. `sub new(...)` is the **constructor**: it runs when a new `Movie` is created.
4. `m.title = title` stores the supplied title on that object.
5. `new Movie("Dune")` creates the object, and `getTitle()` returns its title.

Each `Movie` instance can hold its own title. In the class methods, `m` refers to the current instance.

> BrightScript names are case-insensitive. In this example the variable is called `selectedMovie`, not `movie`, because `movie` would conflict with the class name `Movie` in the same scope.

## 4. Compile the example

You can test the compiler without installing it globally. The commands below use Node.js and npm, and `npx` may download the packages on first use.

From your project's `bright-script` folder, create a separate example folder:

```bash
mkdir -p BrighterScript/hello-brighterscript/source
cd BrighterScript/hello-brighterscript
```

Create `source/main.bs` in that folder and paste the complete class example from section 3 into it. Save it, then run:

```bash
npx --yes --package brighterscript@0.73.8 bsc --files "source/**/*" --create-package false --staging-folder-path "./out/staging"
```

The compiler reads the files under `source`, checks the code, and writes generated files under `out/staging`. This command does not create a Roku package or deploy to a device. A message saying no `bsconfig.json` was found is expected for this small example; the command supplies its options directly.

The files include:

```text
source/main.bs                 ← your BrighterScript source
out/staging/source/main.brs    ← generated BrightScript
out/staging/source/bslib.brs   ← compiler support library
```

Keep your edits in `source/main.bs`, not the generated output. The staging folder is build output and can be replaced when you compile again.

## 5. Run the generated BrightScript locally

From the same `hello-brighterscript` folder, run:

```bash
npx --yes brs@0.45.0 out/staging/source/main.brs
```

Output:

```text
Dune
```

The first command used **`bsc`**, the BrighterScript compiler. This second command uses **`brs`**, the local BrightScript interpreter. The interpreter runs the generated `.brs` file; it is not a Roku device and does not provide all Roku components.

In the generated file, the class is represented using BrightScript functions and an associative array. You can open `out/staging/source/main.brs` in VS Code to inspect it, but the precise generated code may change between compiler versions.

## 6. What comes next?

Classes are a good next topic: we'll look more closely at fields, constructors, methods, and creating multiple objects.

## References

- [BrighterScript project](https://github.com/rokucommunity/brighterscript)
- [BrighterScript classes](https://github.com/rokucommunity/brighterscript/blob/master/docs/classes.md)
- [BrighterScript compiler configuration](https://github.com/rokucommunity/brighterscript/blob/master/docs/bsconfig.md)
