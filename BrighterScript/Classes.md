# BrighterScript 2: Classes and Objects

This lesson continues from [Introduction.md](Introduction.md). A **class** describes the data and behavior of a kind of object. An **instance** is an individual object created from that class.

For example, `Movie` can be a class; one instance might represent `Dune`, while another represents `Coco`.

Each full program below is a separate `.bs` example; section 2 also shows a single-line excerpt. To run one, use the compile-and-run steps from the [introduction](Introduction.md#4-compile-the-example). Save it as `source/main.bs` in a separate example project, or back up your existing example before replacing it. Recompile after each edit: `brs` runs the generated `.brs`, not the `.bs` source.

## 1. Fields and a constructor

A **field** holds a piece of data on an instance. A **constructor** initializes an instance when it is created:

```brightscript
sub Main()
    selected = new Movie("Dune", 2021)
    print selected.title
    print selected.year
end sub

class Movie
    title as string
    year as integer

    sub new(title as string, year as integer)
        m.title = title
        m.year = year
    end sub
end class
```

Output:

```text
Dune
2021
```

`title as string` and `year as integer` declare the fields and their types. `sub new(...)` is the constructor. When `new Movie("Dune", 2021)` is called, the arguments are passed into it and stored on the new instance.

The class is a description; `selected` is the instance created from it.

## 2. What does `m` mean here?

Inside a class method or constructor, `m` refers to the current instance. In the previous example:

```brightscript
m.title = title
```

The left side, `m.title`, is the instance's field. The right side, `title`, is the constructor parameter. This assignment stores the parameter's value in the field. The two names can be the same because `m.title` and `title` refer to different things.

## 3. Create separate instances

Calling `new` twice creates two objects with independent field values:

```brightscript
sub Main()
    firstMovie = new Movie("Dune")
    secondMovie = new Movie("Coco")

    firstMovie.title = "Arrival"
    print firstMovie.title     ' Arrival
    print secondMovie.title    ' Coco
end sub

class Movie
    title as string

    sub new(title as string)
        m.title = title
    end sub
end class
```

Changing the first object's title does not change the second object's title. A field declared this way is accessible through the instance, so `firstMovie.title` can be read or assigned.

## 4. Add methods

A **method** is a sub or function belonging to a class. A method can read or change fields on its current instance:

```brightscript
sub Main()
    selected = new Movie("Dune")
    print selected.getTitle()    ' Dune

    selected.setTitle("Arrival")
    print selected.getTitle()    ' Arrival
end sub

class Movie
    title as string

    sub new(title as string)
        m.title = title
    end sub

    function getTitle() as string
        return m.title
    end function

    sub setTitle(newTitle as string)
        m.title = newTitle
    end sub
end class
```

`getTitle()` returns a string to the caller. `setTitle()` performs an action: it changes the field on the same instance. Like ordinary BrightScript functions and subs, returning a value and printing it are different operations.

## 5. Use methods in decisions

A method can return a Boolean, which you can use in an `if` statement:

```brightscript
sub Main()
    selected = new Movie("Dune", 2021)

    if selected.isRecent() then
        print "Released in 2020 or later"
    else
        print "Released before 2020"
    end if
end sub

class Movie
    title as string
    year as integer

    sub new(title as string, year as integer)
        m.title = title
        m.year = year
    end sub

    function isRecent() as boolean
        return m.year >= 2020
    end function
end class
```

This prints `Released in 2020 or later`. The comparison `m.year >= 2020` produces `true` or `false`; `return` sends that value back to `Main`.

## 6. Assigning an instance does not copy it

Creating a second variable for an existing object does not create a new object:

```brightscript
sub Main()
    firstMovie = new Movie("Dune")
    anotherName = firstMovie

    anotherName.title = "Arrival"
    print firstMovie.title    ' Arrival
end sub

class Movie
    title as string

    sub new(title as string)
        m.title = title
    end sub
end class
```

`firstMovie` and `anotherName` refer to the **same instance**. Compare this with section 3, where each `new Movie(...)` creates a separate instance.

## 7. A class is not a SceneGraph component

A BrighterScript class helps organize code and data. Defining `class Movie` does not create a Roku screen or a visible UI node.

A SceneGraph component is defined separately, usually using XML and a script. Although both class methods and component scripts use `m`, their calling contexts are different. `m.top` is a SceneGraph component-script convention; don't assume it exists inside every class.

## References

- [BrighterScript: Classes](https://github.com/rokucommunity/brighterscript/blob/master/docs/classes.md)
- [Roku: SceneGraph component architecture](https://developer.roku.com/dev/docs/component-architecture)
