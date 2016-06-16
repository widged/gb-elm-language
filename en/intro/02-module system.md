**Elm 0.17**, **shareable** 

# Module System

An Elm program is organized into a collection of modules. Modules help group functionalities that, by some criterion, belong together. For instance, the **Packages** (or **libraries**) that are published to the [package registry](http://package.elm-lang.org/) are all organized into modules.


## Module file

The simplest syntax to declare a module is

```elm
module MyModule exposing (..)
```

A module is stored into a file whose name must begin with a capital letter and exactly match the name of the module they contain. For example, `MyModule.elm` should declare the module `MyModule`. The compiler will not allow module names and file paths to disagree. Convention is to capitalize only the first letter of acronyms (e.g. `Json` and `Http`).

When files are within nested folders, the module contain dots. For example `Json.Encode`, which is in file `Json/Encode.elm`. Folders may be nested as deep as you like, but don't overdo it. 


## Module shape

```elm
module Calculator exposing (..)

import Debug

sum : Int -> Int -> Int
init x y  = x + y
```

### module declaration 

A module declaration begins with the keyword `module`, followed by the module name, the word `exposing` and the list of exported values between parentheses. 

To make explicit that you are exporting everything in this module, use the two dots `(..)`. "Everything" comprises all defined top-level constants, functions, types alias, union types, and their tags.

Use `exposing (..)`.

```elm
module MyModule exposing (..)
```

To limit what is exported, you must list everything that you do want to export (there is no way to "blacklist" private items). A list of items is separated by commas, and may be broken up across many lines. The one wrinkle is how union types are exported, but that will be addressed below.

```elm
module MyModule (MyType, myValue, myFunction) where
```

(source: [elm-for-js](https://github.com/elm-guides/elm-for-js/blob/master/Modules,%20Exports,%20and%20Imports.md))


Although uncommon, you should avoid situations like this:

```elm
module Chance (Model, init) where

type Coin = Heads | Tails

type alias Model = { coin : Coin }

init : Model
init = Model Heads
```

The private `Coin` type is visible in the definition of the public `Model` type. Even worse, someone importing this code can actually obtain a value of type `Coin` through `init`, even though that type doesn't exist to them. And worst of all, the compiler will not catch this error. It *should*, but it doesn't. So be careful when defining types, and think about what will and will not be exported.

(source: [elm-for-js](https://github.com/elm-guides/elm-for-js/blob/master/Modules,%20Exports,%20and%20Imports.md))

## Imports

Elm gives access to a number of values and types by default. At the time of writing, it includes [Basics](http://package.elm-lang.org/packages/elm-lang/core/latest/Basics), Debug, List, Maybe and Result, Program, Cmd, and Sub. You can check the full list of [default imports](http://package.elm-lang.org/packages/elm-lang/core/latest/#default-imports), on the elm-lang website. 

Outside of these, you need to explicitly import from other modules to add useful functionality into your program. For instance, all web applications must import from the _Html_ module. 

```elm
import Html exposing (Html)
```

### import declaration variants

The import declarations specify where a piece of code came from and, optionally, the list of values and types that are directly exposed.

#### qualified imports

The simplest way to import a module is with what is sometimes called a **qualified** import. 

```elm
import String       -- String.toUpper, String.repeat
```

You are then required to always specify (qualify) the module name to access anything from the module, such as `String.toUpper` or `String.repeat`.

#### dump import

When `exposing (..)` is used, it has for effect to dump the entire module into the current one.  All values from the module can be accessed directly. 

```elm
import Html exposing (..)                 -- div, span, h1, h2, etc.
```

This imports the _Html_ module and allows unqualified references to all functions in the module. All node types, `div`, `span`, or `h1` can be used on their own.

#### exposed imports

Yet another option is to explicitly list what is being exposed under unqualified references. 

```elm
import String exposing(toUpper,toLower)
import Html exposing (div, span, h1, h2)
```

For String, access to `toUpper` and `toLower` is direct. All the module-scoped values like are still available, but they must be qualified (`String.repeat` or `Html.text`). 

You can expose multiple values and types from a module by separating them with commas inside the parentheses. The order doesn't matter, but typically the types are listed before the values.


#### When to use exposed?

Two usages are common. 

```elm
import Dict exposing (Dict)
import Html exposing (div, span, h1, h2)
import Json.Encode exposing (object2, (:=))
```

In the first case, the `Dict` module exposes the `Dict` type value in the module (it is very common for modules to define types of the same name as the module itself). It avoids the need to say `Dict.Dict` in you type annotations. 

In the second case, the functions being exposed have distinctive names, and are going to be used frequently.

In the third case, an infix operator is exposed. All infix operators must be imported exposed. Note that infix operators need to be surrounded by extra parentheses. The language does not have syntax for qualified infix operators. Because of this, all infix ops in core are imported exposed by default. Specifically, all of the arithmetic operators in [Basics](http://package.elm-lang.org/packages/elm-lang/core/latest/Basics)  and the cons `(::)` operator of the List module. (adapted from: [elm-for-js](https://github.com/elm-guides/elm-for-js/blob/master/Modules,%20Exports,%20and%20Imports.md))

For any other usage, take the annoyance of having to type such a long word as exposing as a warning to not overuse it. 

Never expose an entire module unless you're really sure about it. When there are multiple `(..)` imports, it becomes impossible to see where something came from, and the odds of a name collision, across modules, is much higher. For instance, for many collection modules, names are deliberately consistent across data structures. For example, the Array, Set, and Dict modules all expose an `empty` value or an `insert` method. The compiler will stop you in truly ambiguous cases. Even in the absence of name collision, the module names serves as a constant reminder of what collection we are using.  (adapted from: [elm-for-js](https://github.com/elm-guides/elm-for-js/blob/master/Modules,%20Exports,%20and%20Imports.md))

#### Renaming an import

When importing a module, you can rename it like so:

```elm
import String as Str                -- Str.toUpper, Str.repeat
import Json.Decode as Decode
import Graphics.Element as Elem exposing (Element, show)
```

As you can see, renaming and exposing can be used together. If you do though, `as Alternative` needs to come before `exposing (the, functions)`.

Renaming is often used by "-extra" packages, which add extra functions to the core library. Typically they will be namespaced according the library they extend, and then imported like this:

```elm
import Random
import Random.Extra as Random
```

Yes, you can rename an import to share a name with another module. In theory, you now no longer need to worry about which functions `Random` exports, and which ones `Random.Extra` does (until you need to look up documentation…). However, some of these libraries will re-export functions in the library they extend, with the same name. Then when you try to use this function, the compiler will error because `Random.map` is ambiguous. There is no way to resolve this error without renaming the import or updating the library.

When you're listing all of your imports, it's helpful to group them in a sensible order. Although you don't have to be neurotic about it, try to put core libraries towards the top, and third-party packages towards the bottom. Modules from the current package or application should be listed last.

(source: [elm-for-js](https://github.com/elm-guides/elm-for-js/blob/master/Modules,%20Exports,%20and%20Imports.md))

#### importing union types

See [Union Type](../type/08-union type.md)


## Export

Exposing happens when you import. This makes it different from exporting, which happens when modules are defined. Many people and even Elm's tooling conflate the two, but if you're being technical they are distinct.


```elm
-- Defining a module, exports everything by default
module Mymodule

-- Export only specified entities
module Mymodule exposing (Type, value) 

-- Export all or specific states of type
module Mymodule exposing
    ( Error(Forbidden, Timeout)
    , Stuff(..)
    )

type Error
    = Forbidden String
    | Timeout String
    | NotFound String
```

(source: [learnyouanelm](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/02-starting-out.md))
