---
layout: post
title: Passing info around in Elm
comments: true
---

As Elm is a functional programming language I'm not sure it's right to talk about 'passing around' objects. It's more 'passing in' your data into the functions. Everything is immutable so there's no danger that something you passed in is going to be changed by the function.

There is also a single data store and a strict reactive cycle in Elm. Everything is represented as being in a state record and each cycle a new copy is in-effect made, with any alteration to the fields specified.

This model is going to get pretty large obvously and in another language you may imagine a reference being passed around and fields being addressed off it.

Something similiar will be happening here. It is not some naive copy of all the information to a new location. The point being that it is not going to be expensive to keep passing the entire model around.

Nevertheless, there are benefits to keeping an eye on what fields are being used or constraining what is passed to a function. 

As was pointed out to me today by [Max Goldstein](https://groups.google.com/forum/#!topic/elm-discuss/_mcAIOQ_whM), this can be done by only passing what is needed as individual parameters, or if everything being passed is a subset of a larger record such as the main model then just the fields within it can be specified.

```elm
pageHome : Model -> Html Msg
pageHome { flags } =
    div [] 
    [ h1 [] [ text "Home" ]
    , img [ src (flags.staticAssetsPath ++ "/Richard.jpeg") ] []
    ]
```

If you want to pass something smaller from your tests then redo the type specification as well. Although if you do it like this you may then need to call it with a record as a parameter, which I guess could be aliased.

```elm
pageHome : { flags : Flags } -> Html Msg
pageHome { flags } =
    div [] 
    [ h1 [] [ text "Home" ]
    , img [ src (flags.staticAssetsPath ++ "/Richard.jpeg") ] []
    ]
```

Interestingly in conjunction with Extensible Records, being more specific like this can also deliver a form of polymorphism. This is the ability to apply your function to multiple record types, so long as the fields you want to use are available in what is passed. In this case the type syntax says what is passed must at least have the fields listed. Like an interface for parameters.

```elm
pageHome : { a | flags : Flags } -> Html Msg
pageHome { flags } =
    div [] 
    [ h1 [] [ text "Home" ]
    , img [ src (flags.staticAssetsPath ++ "/Richard.jpeg") ] []
    ]
 ```
 
You can use this syntax to define type aliases - that may or may not make this more readable.
```elm
type alias FlagsIncludedIn any =
  { any | flags : Flags }

pageHome : FlagsIncludedIn any -> Html Msg
pageHome { flags } =
    div [] 
    [ h1 [] [ text "Home" ]
    , img [ src (flags.staticAssetsPath ++ "/Richard.jpeg") ] []
    ]
```
