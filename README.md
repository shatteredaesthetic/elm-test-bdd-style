elm-test-bdd-style [![Build Status][snap-svg]][snap-url]
==================

[snap-svg]: https://snap-ci.com/rogeriochaves/elm-test-bdd-style/branch/master/build_image.svg
[snap-url]: https://snap-ci.com/rogeriochaves/elm-test-bdd-style/branch/master

BDD style matchers on top of elm-test and elm-check

## Getting started ##

First, follow the getting started steps from the [elm-test](https://github.com/deadfoxygrandpa/elm-test) package.

Then, install elm-test-bdd-style:

```
elm package install rogeriochaves/elm-test-bdd-style
```

To use it, just import it on your test:

```elm
import ElmTestBDDStyle exposing (..)
```

## Usage example ##

```elm
-- Example.elm
module Example where

import ElmTestBDDStyle exposing (..)

tests : Test
tests =
  describe "Example Text"
    [ it "does math correctly" <|
        expect (1 + 1) toBe 2

    , it "does not miscalculate things" <|
        expect (2 + 2) notToBe 5

    , it "exemplifies more complex test cases" <|
        let
          expression = 2 + 2
        in
          expect expression toBe 4
    ]
```

Right now, elm-test-bdd-style comes only with `toBe` and `notToBe`, but it is very easy to create your own matchers:

```elm
toBeGreaterThan : comparable -> comparable -> Bool
toBeGreaterThan = (>)

tests : Test
tests =
  describe "Custom matchers"
    [ it "compares two numbers" <|
        expect 10 toBeGreaterThan 5
    ]
```

## Property Based Testing ##

You can also lean on the power of elm-check in an idiomatic-consise way.

First, check-out [elm-check](https://github.com/NoRedInk/elm-check) page for installing it on your project, you will need the Investigators.

Now just import `Check.Investigator` and you are good to go:

```elm
import ElmTestBDDStyle exposing (..)
import Check.Investigator exposing (..)

tests : Test
tests =
  describe "Property Based Testing"
    [ itAlways "ends up with the same list when reversing twice" <|
        expectEach
          (\list -> List.reverse (List.reverse list))
        toBeTheSameAs
          (identity)
        forEvery
          (list int)
    ]
```

And you will get a hundred runs:

![quick-check](https://cloud.githubusercontent.com/assets/792201/12377802/316d9b88-bd11-11e5-836f-b3b02ba99112.png)
