**Elm 0.17**, **shareable**

Being a functional language, Elm relies heavily on functions. Collection of functions and data types that, by some criterion, belong together can be further organized into modules. For instance, Elm comes with a standard library is organized into modules that each provides access to specialist functionality. For example, the String module for String manipulation operations, the List modules for working with Lists, or the Debug module for logging messages into the console.

Elm pre-built modules commonly capture an abstraction over some data type. The String module or the List module are canonical examples. The module’s functions usually expect an instance of the data abstraction as the first argument. String functions expect a binary string as the first argument, whereas List functions expect a list. Modifier functions return a modified version of the abstraction while query functions as `String.length` or `List.length` tend to return some other type of data.

Though it is common to write modules this way, it is not a requirement. The module is specified completely separately from the type system. The module system is a namespace control mechanism that helps with organizing little groups of related functions under a common umbrella. Nothing more and nothing less.


### Module

To define your own module, you use create a new file that starts with the `module` construct

```elm
module ModuleName exposing (..)
```

The module name must begin with a capital letter and is usually written in CamelCase style. It can consist of alphanumerics, underscores (e.g. `Json` and `Http`).

The module file has to be stored in a file called `ModuleName.elm` (the file name name must exactly match the name of the module they contain and have `.elm` as extension). For example, the module `ModuleName` will be stored in the file `ModuleName.elm`. [:CHECK:] The compiler will not allow module names and file paths to disagree.

To access any definition of the custom module, you have to import the module `import ModuleName` and then call a function with `ModuleName.function_name`.

Often, it is useful to organize modules in some hierarchical fashion. For instance, both Json and Xml data types can be expected to have a Decode or an Encode process. The convention is then to use folders to help create namespaces for a multi-component system and mark each level of folder nesting with a _dot_ (.). For example, a module named `Json.Encode` will be stored in the file `Json/Encode.elm` and a module named `Xml.Encode` will be stored in the path is in file `Xml/Encode.elm`. . The module file must be nested in folders named like the rest of the components and the file name should be the last element of the module name (the part after the last dot). Folders may be nested as deep as you like.

Inside the Elm program, the module name is the one specified in the module declaration. The module `module MyNamespace.MyModule` must be imported with `import MyNamespace.MyModule`.

### Module shape

A module declaration begins with the keyword `module` followed by the module name. In Elm, the module name must be followed by _exposing (exports)_. The _exposing_ part declares what is exposed outside of the current module. It  let you control which of the the functions, types and other values in your module you want to export and make public for consumption elsewhere.

```elm
-- Export everything
module ModuleName exposing (..)

import SomeUtils

type alias Model = Int

add x y  = x + y
convert a = SomeUtils.convert a
```

#### Exporting everything with (..)

When we define the list of exports as `(..)`, we indicate that we are exporting everything in this module. "Everything" comprises all defined top-level constants, functions, types alias, union types, and their tags.

```elm
-- Export everything
module ModuleName exposing (..)

type alias Model = Int

add : Int -> Int -> Int
add x y  = x + y
```

#### Explicitly declaring a public interface

To limit what is exported, you must list everything that you do want to export. The functions, data types and values listed are then the only things defined in the module that can be accessed from outside this module. The list of exports has to be enclosed in parentheses. Export items are separated by commas. For example, the following module exports only specific definitions:

```elm
-- Export only specified entities
module ModuleName exposing (SomeType, someValue, someFunction)

type alias SomeType = ()

someValue = 42

someFunction : Int -> Int -> Int
someFunction x y  = x + y

iAmPrivate = ...
```

Exported items and may be broken up across many lines.

```elm
module ModuleName exposing
  ( SomeType
  , someValue
  , someFunction
  )
```

At this time, it is not possible to "blacklist" items.


### Import statements

Functions and values within a module might depend on functionalities stored in other modules.

Elm gives access to a number of basic types and most used functions by default. For instance, accessors like fst or head. All of the arithmetic operators like `(+)` and the cons `(::)` operator of the List module are also exposed by default. They are all defined in the  [Basics](http://package.elm-lang.org/packages/elm-lang/core/latest/Basics) package.

Elm also gives access to a range of modules without explicity imports. At the time of writing, it includes, Debug, List, Maybe and Result, Program, Cmd, and Sub. You can check the full list of [default imports](http://package.elm-lang.org/packages/elm-lang/core/latest/#default-imports), on the elm-lang website.

As the name suggests, this only covers the basics. To add useful functionality into your program, you are likely to require other modules. For instance, all elm web applications must use the _Html_ module.

Any dependency on a module that is not part of the default imports must be explicitly must be explicitly declared. This is done with import declarations. They specify where a piece of code came from and, optionally, the list of values and types that are directly exposed.

```elm
import Html exposing (Html)
```

Module imports are listed after the module declaration, but before any other definition

There are different ways to import a module:
* dump import with `import Html exposing (..)`
* exposed import with `import Html exposing (Html, div, span, h1, h2)`
* qualified import with `import Html`

#### dump import

The most basic approach is a dump import using `exposing (..)`. It brings into the current scope all public functions from a module, and makes them available for use as if they were defined in the importing module.

```elm
import Html exposing (..)                 -- Html, div, span, h1, h2, text, etc.

main = text "Hello Elm!"
```

This imports the _Html_ module and allows unqualified references to all functions in the module. All node types, `text`, `div`, `span`, or `h1` can be used on their own, without any reference to the module of origin.

#### qualified imports

It is not uncommon for different modules to have definitions of the same name. For instance, for many collection modules, names are deliberately consistent across data structures. Both `Dict` and `List` modules have a `filter` function. The Array, Set, and Dict modules all expose an `empty` value or an `insert` method. With a dump import, the compiler has no way to know which collection type to use.

A better approach is to control exactly which definitions you to import. To do so, you must include a list of desired elements in a list surrounded by parentheses. This is sometimes called a **qualified** import.

```elm
import String      -- String.toUpper, String.repeat
import List        -- List.List, List.filter
import Dict        -- Dict.Dict, Dict.filter
```

With qualified imports any type, value, or function must be prefixed with the name of the module it came from, such as `String.toUpper` or `String.repeat`.

#### exposed imports

Yet another option is to explicitly list, inside the parentheses, what is being exposed under unqualified references. The exposed items are separated with commas. The item order doesn't matter, but typically the types are listed before the values.

```elm
import String exposing(toUpper,toLower)
import Html exposing (Html, div, span, h1, h2)
import List exposing (List)
import Dict exposing (Dict)
```

Once the listed declarations are imported, they can be accessed directly. For String, access to `toUpper` and `toLower` is direct. Importantly, you can still access the module other public definitions  with the qualification of the module (`String.repeat` or `Html.text`). Those concepts are orthogonal, and you can combine them freely.


#### aliased imports

In some cases, the name of a module is too long to be used as a prefix. To save endless typing, you can rename the module using `as`. Afterward you prefix the declarations with the alias.

```elm
import String as Str                -- Str.toUpper, Str.repeat
import Json.Decode as Decode
```

At any time, you can mix qualified imports with renaming and explicit import lists.

```elm
import Graphics.Element as Elem exposing (Element, show)
```

Aliases can be useful if a module has a long name. For example, let’s say you have a Geometry.Rectangle module. You can alias it in your client module and use a shorter name `Rect`.

Aliasing and exposing can be used together. If you do though, `as Alternative` needs to come before `exposing (the, functions)`.

Renaming is often used by "-extra" packages, which add extra functions to the core library. Typically they will be namespaced according the library they extend, and then imported like this:

```elm
import Random
import Random.Extra as Random
```

### Guidelines

#### don't overuse dump imports

Never expose an entire module unless you're really sure about it. When there are multiple `(..)` imports, it becomes impossible to see where something came from, and the odds of a name collision, across modules, is much higher. In contrast, the module qualification serve as a constant reminder of the data structure that the definition relates to.

#### limit unqualified access

Take the annoyance of having to type such a long word as exposing as a warning to not overuse it. As a rule, exposed items should be limited to the type abstraction of the module (typically, with the same name as the module itself) and functions that are going to be used frequently, provided that they have distinctive names.

```elm
import Dict exposing (Dict) -- avoids the need to say `Dict.Dict` in type annotations
import Html exposing (div, span, h1, h2)
```

#### expose any infix operators

As the elm language does not have syntax for qualified infix operators, they must all be exposed. Note that they need to be surrounded by extra parentheses.

```elm
import Json.Encode exposing (object2, (:=))
```

#### import Extra functions under the name of the module extended

Modules that provide extra functionalities to an existing module are suffixed with `Extra`. For instance, modules `Random` and `Random.Extra`. It is customary to import both under the same name.

```elm
import Random
import Random.Extra as Random
```

You can then use functions from either module. Libraries must avoid to export functions of the same name. If matching names are used, the compiler will error because `Random.map` is ambiguous.

#### exposing Data Types

Data types need some extra syntax for being selected for import. Two pieces of information are needed: the type itself, and its constructors `Type(List of Constructors)`. Here are several ways in which we can import the Client data type from the first section:

With data types, you can fine tune the control of the export of the data type and type constructors. You have several options, for either import or export:

exports:

```elm
module ModuleName exposing ( Msg(..) )       -- all constructors
module ModuleName exposing ( Msg(Change) )   -- a subset of constructors
module ModuleName exposing ( CardSuit() )    -- only type, no constructors
```

imports:

```elm
import ModuleName ( CardSuit() )    -- only type, no constructors
import ModuleName ( Msg(Change) )   -- a subset of constructors
import ModuleName ( Msg(..) )       -- .. is used for importing all states of type
```

On export, opting for an empty list will disallow the creation of values by directly calling the constructors.

See [Union Type](../type/08-union type.md)

#### ordering imports

Try to put core libraries towards the top, and third-party packages towards the bottom. Modules from the current package or application should be listed last.

### Further Reading

* [learnyouanelm](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/02-starting-out.md)
* [elm-for-js](https://github.com/elm-guides/elm-for-js/blob/master/Modules,%20Exports,%20and%20Imports.md)
