# BrightScript Fundamentals 10: `m` and Roku Components

This lesson continues from [Debugging.md](Debugging.md). Roku apps use **SceneGraph components** to build TV screens. These components often use a special variable called `m` to keep track of their state and UI nodes.

## 1. What is `m`?

`m` is a special associative array available inside BrightScript functions. What it refers to depends on the calling context. For example, when a function is called as an associative array's method, `m` refers to that associative array:

```brightscript
sub Main()
    first = { name: "Home", show: showName }
    second = { name: "Movies", show: showName }

    first.show()
    second.show()
end sub

sub showName()
    print m.name
end sub
```

Output:

```text
Home
Movies
```

Both objects use the same `showName` sub, but `m` refers to the object making each call. This standalone example can run locally with `npx brs filename.brs`.

## 2. What is a SceneGraph component?

A SceneGraph component is a reusable part of a Roku app's UI, such as a screen or a custom control. It is commonly described by two files:

- An **XML** file defines the component and its child nodes.
- A **BrightScript** file contains its behavior.

For example, an XML file can contain a `Label` node that displays text, while its BrightScript file sets that label's text. A `Scene` is a component that can serve as a screen.

## 3. Define a scene and a label

This example is a component pair, **not a complete app**. It needs a Roku app to create and display the scene on a Roku device; `npx brs` alone cannot display it.

`components/LessonScene.xml`:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<component name="LessonScene" extends="Scene">
    <script type="text/brightscript" uri="pkg:/components/LessonScene.brs" />
    <children>
        <Label id="messageLabel" text="Starting..." translation="[80, 100]" />
    </children>
</component>
```

- `extends="Scene"` makes this component a scene.
- The `script` element links the BrightScript file inside the app package. `pkg:/` refers to the package root.
- `id="messageLabel"` gives the label a name the script can look up.
- `translation` sets its position, in pixels, relative to its parent.

## 4. Find and update a node

`components/LessonScene.brs`:

```brightscript
sub init()
    m.messageLabel = m.top.findNode("messageLabel")

    if m.messageLabel <> invalid then
        m.messageLabel.text = "Hello from BrightScript!"
    end if
end sub
```

In a SceneGraph component script:

- `init()` is called when the component is initialized.
- `m.top` refers to the component node itself. `findNode("messageLabel")` searches its descendants for the ID.
- `m.messageLabel` stores the found label in this component's `m`, so another function in the same component can use it.
- `m.messageLabel.text` changes the label's displayed text. The `invalid` check avoids accessing `.text` if the node was not found.

If the scene is created and shown in an app, its label should display `Hello from BrightScript!`. Each component instance has its own state; don't assume that `m.messageLabel` in one component is automatically available in another.

## 5. Component state and node fields

In the example, `m.messageLabel` is a reference the script stores in `m`. The label's `text` is a **field on the label node**. They are related, but not the same thing:

```text
m.messageLabel        -> the label node found by the script
m.messageLabel.text   -> the label node's text field
```

Keeping a reference in `m` means you don't have to look up the node again every time another function in that component needs it.

## 6. What happens next?

To show this scene, a complete Roku app also needs a `manifest` and a `source/main.brs` that creates a SceneGraph screen and scene. SceneGraph behavior must be checked on a Roku device; success in the local `brs` interpreter does not establish that the UI runs there.

## References

- [Roku: SceneGraph component architecture](https://developer.roku.com/dev/docs/component-architecture)
- [Roku: Program statements](https://developer.roku.com/dev/docs/program-statements)
- [Roku: SceneGraph developer guide](https://developer.roku.com/dev/docs/scenegraph)
