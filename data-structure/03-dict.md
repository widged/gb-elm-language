**Elm 0.??**, **copied, please ignore** 

# Dicts

## What is a Dictionary?

A dict maps unique keys to values. There’s a fair amount of overlap in the use cases for dictionaries and sets, but dictionaries allow for storing a bit more information. Since values are indexed by unique keys, we can try to Dict.get 'some comparable' Dict.empty, which may give us Nothing.

(source: [Data Structures in Elm @NoRedInk](http://tech.noredink.com/post/140646140878/data-structures-in-elm))

## Our Library Has a Dictionary

Our library book loaning system from the set example might work better as a dict. With a dict, we can tie checked out ids to library entries directly.

```elm
type alias LibraryEntry =
    { id : LibraryEntryId
    , name : String
    , author : String
    , readByMe : Bool
    }

type alias LibraryEntryId =
    String

model =
    { yourLibraryBasket = Dict.empty -- Dict.empty constructs an empty dict
    , checkedOutBooks = Dict.empty
    }

henryV = LibraryEntry "0" "Henry V" "Shakespeare" True
```

(source: [Data Structures in Elm @NoRedInk](http://tech.noredink.com/post/140646140878/data-structures-in-elm))

Now, suppose we want to add Henry V to our library basket: Dict.insert henryV.id henryV model.yourLibraryBasket will give us back a dictionary with a key “0” pointing to a LibraryEntry describing the play Henry V. This can make it very easy to work with henryV.

Suppose we just want to display books that are in our basket. When we were using a set to hold the ids of our books, this would have required some filtering/checking every book id to see if it was in the set of basket ids or not. With a dict, we can do something similar, (Dict.member "0" model.yourLibraryBasket == True), but we can also just grab all of the values in our Dict:

```elm
type alias Model =
    { yourLibraryBasket : Dict LibraryEntryId LibraryEntry -- Note the type signature here
    , checkedOutBooks : Dict LibraryEntryId LibraryEntry
    }


model =
    -- Dict.singleton creates a dictionary with 1 key-value pair. henryV is this value.
    Model (Dict.singleton henryV.id henryV) Dict.empty


viewBook : LibraryEntry -> Html
viewBook {name, author, readByMe} =
    div
        [ classList [ ("is-unread", readByMe) ] ] {- classList takes List (String, Bool),
                                                     where each tuple represents a className and whether to apply it -}
        [ div [ class "book-name" ] [ text name ]
        , div [ class "book-author" ] [ text author ]
        ]


viewBookList : List (LibraryEntry) -> Html
viewBookList bookList =
    bookList
        |> List.map viewBook
        |> div [ class "book-collection-container" ]

viewLibraryBasket : Model -> Html
viewLibraryBasket model =
    let
       yourBasket =
            model.yourLibraryBasket
                |> Dict.values -- This gets us just the values in a list
                |> viewBookList
    in
        div
            [ class "basket" ]
            [ h3 [] [ text "Your Basket" ]
            , yourBasket
            ]
```

Now, how do we remove books from our basket? The semantic `Dict.remove "0" model.yourLibraryBasket`. Let’s write out a function that will either remove books from our basket or add them to our basket, on a case-by-case basis.

```elm
type Action
    = RemoveFromBasket LibraryEntryId
    | AddToBasket LibraryEntry


update : Action -> Model -> Model
update action model =
    case action of
        RemoveFromBasket libraryEntryId ->
            { model | yourLibraryBasket = Dict.remove libraryEntryId model.yourLibraryBasket }

        AddToBasket libraryEntry ->
            { model | yourLibraryBasket = Dict.insert libraryEntry.id libraryEntry model.yourLibraryBasket }
```

(source: [Data Structures in Elm @NoRedInk](http://tech.noredink.com/post/140646140878/data-structures-in-elm))