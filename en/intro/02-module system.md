**Elm 0.??**, **copied, please ignore** 

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

You can import from other modules to add useful functionality into your program. All web applications will import from the _Html_ module. Other programs might import from the _Dict_ module.

```elm
import Dict exposing (Dict)
```

The `Dict` in parentheses refers to the type, not the module. All the module-scoped values like `Dict.insert` are still available. You can expose multiple values and types from a module by separating them with commas inside the parentheses. You can find more details in [this guide](Modules, Exports, and Imports.md), but this practice in general is discouraged. (This is why the language forces you to type the long `exposing` keyword.)

Elm also imports some values and types by default. The full list is [here](http://package.elm-lang.org/packages/elm-lang/core/latest/#default-imports), but the most important thing to know is that all of [Basics](http://package.elm-lang.org/packages/elm-lang/core/latest/Basics) is imported exposed. The List, Maybe, and Signal modules and types are also available to you without an explicit import.

(source: [elm-for-js](https://github.com/elm-guides/elm-for-js/blob/master/Modules,%20Exports,%20and%20Imports.md))


### import declaration

Next, come the import declarations. They  specify where a piece of code came from.

(source: [learnyouanelm](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/02-starting-out.md))

#### qualified import

```elm
-- qualified imports
import String                       -- String.toUpper, String.repeat
import String as Str                -- Str.toUpper, Str.repeat
```

`import String` imports the _String_ module, and allows qualified references to functions in the module, such as String.toUpper.


The most common way to import a module is also the simplest:

```elm
import Dict
```

This gives you access to everything in the [Dict library](http://package.elm-lang.org/packages/elm-lang/core/latest/Dict) by prefacing it with the module name and a dot. This is sometimes called a **qualified** import. So if you want to use `Dict.insert`, this import statement is sufficient.

Qualified imports are the preferred way to import values from other modules, because you can always tell where something came from by the module name right in front of it. Conversely, it makes it easy to tell what is defined in *this* module.

Additionally, for many collection modules, names are not unique across modules. In fact, they are deliberately consistent. For example, the Array, Set, and Dict modules all expose an `empty` value, so the module names help you tell them apart.

(source: [elm-for-js](https://github.com/elm-guides/elm-for-js/blob/master/Modules,%20Exports,%20and%20Imports.md))

#### exposed import

```elm
-- unqualified imports
import Mymodule exposing (..)                 -- Error, Stuff
import Mymodule exposing ( Error )            -- Error
import Mymodule exposing ( Error(..) )        -- Error, Forbidden, Timeout
import Mymodule exposing ( Error(Forbidden) ) -- Error, Forbidden
```

`import String exposing(toUpper,toLower)`imports the _String_ module, and allows unqualified references to _toUpper_ and _toLower_, and qualified references to other functions in the module.

`import String exposing(..)` imports the _String_ module and allows unqualified references to all functions in the module.

(source: ???)


The alternative to a qualified import is an **exposed** import, because of the keyword `exposing` (whose length should serve as a deterrent to using it!). But it's usually okay if you list what you're exposing, for example:

```elm
import Dict exposing (Dict)
import Html exposing (div, span, h1, h2)
```

In the first case, the `Dict` in parentheses is a type, not the module. It's very common for modules to define types of the same name as the module itself. It avoids the need to say `Dict.Dict` in you type annotations. In the second case, the functions being exposed have distinctive names, and are going to be used frequently.

In both cases, the module name is also available to be use qualified. So you can still use `Dict.insert` or `Html.text`. When you have a list of exposed imports, the order doesn't matter, but typically the types are listed before the values.

(source: [elm-for-js](https://github.com/elm-guides/elm-for-js/blob/master/Modules,%20Exports,%20and%20Imports.md))

#### dump import

The real reason exposed imports are considered an antipattern is because they can be combined with `(..)` to dump the entire module into the current one. When there are multiple such imports, it becomes impossible to see where something came from, and the odds of a name collision is much higher.

```elm
-- Antipatten!
import Dict exposing (..)
```

`Dict.insert` will now be available as `insert`. This can easily be confused with other insert operations, although the compiler will stop you in truly ambiguous cases. So don't expose an entire module unless you're really sure about it.

Regardless, exposing happens when you import. This makes it different from exporting, which happens when modules are defined. Many people and even Elm's tooling conflate the two, but if you're being technical they are distinct.

(source: [elm-for-js](https://github.com/elm-guides/elm-for-js/blob/master/Modules,%20Exports,%20and%20Imports.md))

#### Special cases

Infix operators must be imported exposed, for example as `import Json.Encode exposing (object2, (:=))`. Note that infix operators need to be surrounded by extra parentheses. The language does not have syntax for qualified infix operators. The good news is that, with the exception of the previous example, all infix ops in core are imported exposed by default.

Specifically, all of the arithmetic operators are in
[Basics](http://package.elm-lang.org/packages/elm-lang/core/latest/Basics), which is imported exposed automatically.
(It's worth becoming familiar with everything in that module.) List cons, `(::)`, is imported along with the `List` type, as are `Maybe` and its tags, `Just` and `Nothing`.

(source: [elm-for-js](https://github.com/elm-guides/elm-for-js/blob/master/Modules,%20Exports,%20and%20Imports.md))

#### Renaming an import

When importing a module, you can rename it like so:

```elm
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

#### Exporting and Importing Union Types

Union types have a little bit of special treatment when it comes to imports and exports. Let's start with a simple union type, which you might find in a counter demo.

`type Action = Increment | Decrement`

Remember that `Increment` and `Decrement` are called "tags". How would we export this type?

`module MyModule exposing (Action)` exports `Action` only. That is called an *opaque type*. Clients can see that `Action` exists, but they can't see into it. This is frequently what you want, and we'll talk about it more in the next section.

`module MyModule exposing (Action(..))` exports `Action` and all of its tags, namely `Increment` and `Decrement`. This form can be useful sometimes if you can commit to keeping `Action` the same. A good example is a [nonempty list](http://package.elm-lang.org/packages/mgold/elm-nonempty-list/latest/List-Nonempty#Nonempty); there will never be a reason to change the type's definition so the tag can be safely exported.

`module MyModule (Action(Increment, Decrement)) where` exports `Action` and only the listed tags. In this case it's listing all the tags explicitly. It's possible to only export *some* of the tags, but there is no reason to do this, ever.

The trouble with exporting tags is not only that you may want to remove some, which will break any code that relies on the ones being removed. Even adding tags will break code, because previously exhaustive pattern matches are no longer exhaustive. If only some of the tags are exported, it's impossible to write a valid `case` statement (at least not without a `_ ->` pattern, which are discouraged).

Importing union types exposed follows the exact same syntax. For example, `import MyModule exposing (Action)` will import only the type, while `import MyModule exposing (Action(..))` will import any exported tags as well. You can also use exported tags qualified, like `MyModule.Increment`.

(source: [elm-for-js](https://github.com/elm-guides/elm-for-js/blob/master/Modules,%20Exports,%20and%20Imports.md))

#### Opaque Types

An **opaque type** is a union type where the type is exported but the tag(s) are not. Someone outside the module can see that the type exists, pass it around, and store it in records. They only thing they *can't* do is look inside the type, even if they know what the tags are named. Hence, it's opaque.

An example of an opaque type would be a 2D point. Creating a point would require either `x` and `y`, or `r` and `theta`. Each value could be accessed individually from a given point. The point might actually store all four, knowing that there's no way for someone to create a point that's inconsistent.

What this means is that opaque types are Elm's way of enforcing information hiding. They allow a package author to define an interface of functions to create, update, and read useful values out of the opaque type. The implementation can change completely, but as long as all functions on the type are updated to match, it's still considered a patch change. This gives package writers flexibility when writing their libraries. It also lets them rely on invariants, assured that the client hasn't meddled with values of the type in unexpected ways.

Opaque types are less useful in applications. If you're typing to simply pass information around, exporting record type aliases is fine. If it makes sense to also define operations on these models, an opaque type might be a better fit.

If your union type contains many tags, you can export functions that wrap them. You can be selective about which ones you export. Sometimes it can be tedious, but it's worth it.

```elm
module Road (Action, addCar, redLight) where

import Car exposing (Car)

type Light =
  Red | Yellow | Green

type Action =
  AddCar Car | Light Light | SomethingPrivate

addCar : Car -> Action
addCar =
  AddCar

redLight : Action
redLight =
  Light Red
```

You cannot make an opaque type out of a type alias; those are either exported or not, just like values. But you can create a union type to hide it. (This is more common when the opaque type represents a model rather than an action.)

```elm
module Person (Person, age) where

type Person =
  P { name : String, age : Int }

age : Person -> Int
age (P {age}) =
  age
```

First, we define the union type `Person` with one tag, `P`, which carries a record. (You can put the record definition directly in the union type, or define an unexported alias.) Then we can export the `age` function which accesses the record in a way not possible outside of this module. The definition uses two nifty language features. First, it pattern matches on the `P` tag in the argument list. This is permitted when there is only one tag, because otherwise it's an incomplete pattern match. Next, `{age}` destructures the record, assigning the local constant `age` to the value of the record's `age` field. It's a much more concise way of writing this:

```elm
age person =
  case person of
    P record ->
      record.age
```

(source: [elm-for-js](https://github.com/elm-guides/elm-for-js/blob/master/Modules,%20Exports,%20and%20Imports.md))

## Export

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
