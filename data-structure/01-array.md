**Elm 0.??**, **copied, please ignore** 

# Arrays

Arrays are ordered collections, where each item in the collection must be of the same type. In Elm, [] doesn’t correspond to arrays ([] is an empty list, discussed above). To create an empty array, use Array.empty.

To create an Array of a given length with a specific default element, do: Array.repeat 9 "Tessa". This will give you an Array filled with 9 “Tessa”’s. Some might say this is too many. A third option is to use Array.initialize, whose first argument is the desired length of the array. Array.initialize’s second argument is a function expression operating on the index; the result of this operation is the value of the array at that index.

```elm
arrayOfTessas =
    Array.initialize 9 (\index -> "Tessa" ++ (toString index))
```

Then arrayOfTessas will be an Array that in JavaScript-land looks like: ["Tessa0","Tessa1","Tessa2","Tessa3","Tessa4","Tessa5","Tessa6","Tessa7","Tessa8"].

Okay, sweet. But obviously “Tessa3” is my favorite, and the one that I want. How do I get it?

(source: [Data Structures in Elm @NoRedInk](http://tech.noredink.com/post/140646140878/data-structures-in-elm))

In JavaScript, you get a value back for any index or key you ask for in an array, since an array is just an object. (In JavaScript, arr["Something I want"] tends to be undefined, but what can you do.)

In Elm, accessing a value at an index that doesn’t exist will fail explicitly–there’s no question of whether you got undefined because you were doing something marvelous with your array like [0,1,undefined,"Bwahaha that's going to confuse me later"]. Instead, in Elm, you will get back Nothing if there was nothing at that index (this is a Maybe).

Suppose I have an array comprised of my LibraryEntrys from before.

```elm
myBooks : Array LibraryEntry
myBooks =
    Array.fromList [ wutheringHeights, diaspora ]
    {- For convenience, we're using Array.fromList to convert this list of library entries into an array. -}
```

Suppose I try to get the value at the second index. This is going to fail, since arrays’ indexing starts at 0, and the length of the array is 2. But I try it anyway: `myBooksSecondIndex = Array.get 2 myBooks`. So what’s `myBooksSecondIndex`? It’s Nothing.

So maybe you’re thinking that there are situations where you might have `Nothing` in an Array. That’s fine too.

```elm
maybeMyBooks : Array (Maybe LibraryEntry) -- Note that the type signature indicates that the elements are Maybes
maybeMyBooks =
    Array.fromList [ Just wutheringHeights, Just diaspora, Nothing ]
```

Now, if we try to access the second index: myBooksSecondIndex = Array.get 2 maybeMyBooks we have myBooksSecondIndex == Just Nothing. This is unlikely to be a useful way of structuring things, and I’d encourage you to stahp, but it does illustrate the clarity that Elm brings to a familiar JavaScript problem: is that undefined because it’s supposed to be, or because I failed to set a value?

Speaking of setting values: we can push a value to the end of the array, append an Array to another to create a new array, or we can set a value at an index. Setting values is a bit different than in JS.

In JavaScript, I can do something like this:

```elm
var aRiddle = "What's the longest word in the English language?"

var myAnswerArray = []

// Hmmmmmmmmmmmm

myAnswerArray.push("S")
myAnswerArray[5279] = "S"

console.log(myAnswerArray.length + " feet") // => "5280 feet"

var myAnswer = "Smiles! There's a mile in the middle!"
```

In Elm, not so much.

```elm
myAnswerArray =
    Array.fromList ["S"]
        |> Array.set 5279 "S"
```

Array.set will not update the Array it’s passed if the given index is out of bounds. We tried to set “S” as a value at index 5279, but that’s way out of bounds, so this set will just not occur. Array.toList myAnswerArray is just ["S"]. No mile in the middle here!

(source: [Data Structures in Elm @NoRedInk](http://tech.noredink.com/post/140646140878/data-structures-in-elm))

### Array Implementation

Arrays are implemented as relaxed-radix balanced trees. They’re an extension to vectors (as used in Clojure and Scala), but with optimized concatenation. Effectively, even though they’re immutable, they have constant time getting and setting. Obviously, this is magic (read: math). 

To read the math and learn the magic: _Bagwell and Rompf’s “RRB-Trees: Efficient Immutable Vectors”_

To read more about Elm’s implementation, read this blog announcement.

As cool as the math behind arrays is, and as familiar as they might seem, we tend not to use them. Currently, there are reliability issues with arrays and many Elm developers tend toward using lists instead. But fear not! Array is being rewritten, in pure Elm instead of Native. Be excited.

(source: [Data Structures in Elm @NoRedInk](http://tech.noredink.com/post/140646140878/data-structures-in-elm))

## Links

* [Elm 0.12.1 -- Fast, Immutable Arrays](http://elm-lang.org/blog/announce/0.12.1)
