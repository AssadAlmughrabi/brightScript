# BrighterScript Lesson: Namespaces and Imports

## Part 1: Namespaces

A namespace groups related functions, classes, enums, and constants under one prefix. Plain BrightScript has no such grouping, so every function lives in one global pool. Two files that each define `format()` collide. Namespaces fix that.

### Basic example

```brighterscript
namespace Utils
    function formatTitle(title as string) as string
        return ucase(left(title, 1)) + lcase(mid(title, 2))
    end function

    function clamp(value as integer, low as integer, high as integer) as integer
        if value < low then return low
        if value > high then return high
        return value
    end function
end namespace

sub Main()
    print Utils.formatTitle("hELLO")    ' Hello
    print Utils.clamp(15, 0, 10)        ' 10
end sub
```

### What it transpiles to

The dots become underscores. Check this yourself in the BrighterScript playground:

```brightscript
function Utils_formatTitle(title as string) as string
    ...
end function

sub Main()
    print Utils_formatTitle("hELLO")
end sub
```

Namespaces are a **compile-time feature**. At runtime there is no `Utils` object, just functions with long names.

### Rules to remember

1. **Siblings call each other without the prefix** inside the same namespace:

   ```brighterscript
   namespace Utils
       function isBlank(s as dynamic) as boolean
           return s = invalid or s = ""
       end function

       function safeTitle(s as dynamic) as string
           if isBlank(s) then return "Untitled"   ' no Utils. needed
           return formatTitle(s)
       end function
   end namespace
   ```

2. **Nesting** uses dots: `namespace Utils.Strings` is called as `Utils.Strings.trim()`.
3. **Classes, enums, and consts** can live in namespaces too:

   ```brighterscript
   namespace Models
       class Movie
           title as string
           year as integer

           sub new(title as string, year as integer)
               m.title = title
               m.year = year
           end sub
       end class
   end namespace

   movie = new Models.Movie("Inception", 2010)
   ```

4. **The same namespace can be split across files.** Each file adds to it.
5. Namespace names are case-insensitive, like everything in BrightScript.

---

## Part 2: Imports

`import` tells the compiler that this file depends on another file.

```brighterscript
import "pkg:/source/utils.bs"
```

The path starts with `pkg:/`, which is the package root (your `rootDir`), not the current folder.

### Where imports matter

| Location | Behavior |
|---|---|
| `source/` (main scope) | All files in `source/` already share one scope, so imports there mostly document dependencies. |
| A SceneGraph component (`components/`) | Each component has its **own scope**. It only sees what it imports (or what its XML includes). |

### Using it in a component

```xml
<!-- components/MovieList.xml -->
<component name="MovieList" extends="Group">
    <script type="text/brighterscript" uri="MovieList.bs" />
</component>
```

```brighterscript
' components/MovieList.bs
import "pkg:/source/utils.bs"

sub init()
    print Utils.formatTitle("my list")
end sub
```

At build time, BrighterScript turns each import into a `<script>` tag in the generated XML. That is how the component gets access to the function.

### The most common error

If you see "cannot find name `Utils`" or "class not found" inside a component, **a missing `import` is almost always the cause**. It is easy to miss because the same code works in `source/`.

