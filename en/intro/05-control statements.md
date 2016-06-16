**Elm 0.17**, **shareable** 

# Control statements

## If statements

```elm
if condition then value_if_True else value_if_False
if | condition -> expression | condition -> expression ...
| otherwise -> expression
case expression of pattern -> expression ...
pattern -> expression
```

If statements always have an else, and the branches must be the same type.

``elm
if powerLevel > 9000 then
"WHOA!" else
"meh"
```

If statements can be chained.

``elm
if n < 0 then
  "n is negative"
else if n > 0 then
  "n is positive"
else
  "n is zero"
```

(source: ???)

All the branches of an if need to match so that no matter which one we take, we get back the same type of value overall.
```elm
if a < 1 then
    "It's zero"
else
    "Non-zero"

-- Multi-line.
if y > 0 then
    "Greater"   
else if x /= 0 then
    "Not equals"
else
    "silence"
```    

Elm does not have the notion of “truthiness”.<br/>
The condition must evaluate to True or False, and nothing else.
```elm
> if 1 then "nope" else "nope again"
- TYPE MISMATCH --
```

(source: [learnyouanelm](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/02-starting-out.md))

## Case-of statements

Use case statements to pattern match on different possibilities.

```elm
case aList of
  [] -> "matches the empty list"
  [x]-> "matches a list of exactly one item, " ++ toString x
  x::xs -> "matches a list of at least one item whose head is " ++ toString x
```

Pattern matches go in order. If we put [x] last, it would never match because
x::xs also matches (xs would be the empty list). Matches do not "fall through".
The compiler will alert you to missing or extra cases.
Pattern match on a Maybe.

```elm
case List.head aList of
  Just x -> "The head is " ++ toString x
  Nothing -> "The list was empty."
```

(source: ???)


Case tries to match the value of type against patterns defined after the `of` keyword
```elm
type User
    = Activated
    | Deleted

update state =
  case state of
    Activated ->
      -- do something
    Deleted ->
      -- do again
```

In case of passing tags with additional properties, parameters are passed along with type checking
```elm
type User
    = Activated Int
    | Deleted (Int, String)

update state =
  case state of
    Activated value ->
      -- do something with value
    Deleted values ->
      -- do something with values

update ( Activated 1 )
update ( Deleted (0, "gone") )
```

(source: [learnyouanelm](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/02-starting-out.md))

## Let-in

`let` allows you to define intermediate values.
```elm
let
  x = 3 * 8
  y = 4 ^ 2
in
  x + y
```

`let` helps simplify complex expressions
```elm
let
  activeUsers = List.filter (\u -> u.state /= 1) model.users
in
  { model | user = activeUsers}
```

(source: [learnyouanelm](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/02-starting-out.md))