**Elm 0.17**, **shareable**

## Comments

Comments can include any kind of text and are completely ignored by both interpreter and compiler (although some tools like package publish get information from the comments).

```elm
-- Single line comments start with two dashes. The comment spans to the next new line character.
{-
  Multi-line comments are enclosed in a comment block
-}
{- Multiline comments
  {- can be nested. -}
-}
a = 1 -- comments can be added
b = 2 {- after some some text -}
```
