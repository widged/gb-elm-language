**Elm 0.??**, **copied, please ignore** 

## Sets

### What are sets, and when should you use them?

Elm’s sets are a very valuable data structure, and one that’s worth knowing well. All the basic functionality of sets will be familiar from middle and high school math (also, college level, if you took Analysis or Real Analysis or Abstract Algebra or Logic or Philosophy). A set is a collection of unique items; in Elm, these items must be comparables (ints, floats, chars, strings, lists, or tuples) and of the same type.

So when might it make sense to use a set? Whenever you find yourself doing a lot of filtering to check for List entries’ inclusion in other Lists, you should think about using Sets instead. Is your collection of elements closed? Is your collection of elements a subset of another collection? Should the elements of your collection be unique? Here too you may want to use a Set instead of a List.

In deciding to use a set rather than another data structure, there’s one other important consideration: are the elements of your collection comparable? Sets can only contain comparable values. So our collection of sweet library books? Not going to work as a set.

### Using a Set

Suppose that our LibraryEntrys had unique ids. We could store a string id in a set.

```elm
type alias LibraryEntry =
    { id : LibraryEntryId
    , name : String
    , author : String
    , readByMe : Bool
    }

type alias LibraryEntryId =
    String
```

With these ids, we can build a library basket (because library baskets are definitely a thing don’t worry about it there are totally baskets) and a collection of checked out books.

```elm
model =
    { yourLibraryBasket = Set.empty -- Set.empty constructs an empty set
    , checkedOutBooks = Set.empty
    }
```

Now, checking to see if a book (a LibraryEntry with, say, id "10") is checked out is simple: `Set.member "10" model.checkedOutBooks` returns a boolean describing whether or not “10” is an element in `model.checkedOutBooks`. Is the book already in our basket? `Set.member "10" model.yourLibraryBasket` will tell us.

(source: [Data Structures in Elm @NoRedInk](http://tech.noredink.com/post/140646140878/data-structures-in-elm))

### Implementation

Sets are actually just a wrapper around Elm’s Dicts, which are implemented with a red black tree. Be aware that as a result of this implementation, == is not reliable (with Sets or Dicts). The order in which elements are added to a RB-tree impacts the form the tree takes, which makes equality a sticky concept.

Let’s walk through an example.

```elm
set1 = Set.fromList ([1,2,3,4])

set2 = Set.fromList ([1,2,3,4])

set3 = Set.fromList ([4,3,2,1])
```

We have three sets, each with the same values: 1, 2, 3, and 4. The only difference between the Sets is in the order in which these values are added. For these sets, does the root node have value 2 or 3? Who knows?

```elm
listVersionsAreOrdered =
    Set.toList set1 == Set.toList set2 && Set.toList set2 == Set.toList set3 -- True

setsCreatedInSameOrderAreEqual =
    set1 == set2 -- True

uhOhNotEqual =
    set2 == set3 -- False
```

Moral: don’t ask whether sets are equal, because what you’re asking is “are these two trees made out of the same values && were these trees balanced in the same way?” It’s worth noting that this warning about equality might not always apply. Current plans for Elm include changing the equality behavior for sets and dicts to not be dependent on the balancing of an RB-tree. For now, the documentation provides a warning.

(source: [Data Structures in Elm @NoRedInk](http://tech.noredink.com/post/140646140878/data-structures-in-elm))