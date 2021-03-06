# Homework Week 4

**Decode User**

```elm
import Json.Decode as J

type alias User =
    { name : String
    , cats : Maybe Int
    }

decodeUser : J.Decoder User
decodeUser = Debug.todo ""

> J.decodeString decodeUser "{\"name\": \"Kostia\", \"cats\": 2}"
Ok { cats = Just 2, name = "Kostia" }
    : Result J.Error User
> J.decodeString decodeUser "{\"name\": \"Droopy The Dog\", \"cats\": null}"
Ok { cats = Nothing, name = "Droopy The Dog" }
    : Result J.Error User
```

**Mottos**

```elm
mottos : String
mottos =
    """
      {"Germany": {"motto": "Einigkeit und Recht und Freiheit", "currency": "EUR"},
       "England": {"motto": "God Save the Queen", "currency": "GBP"},
       "France": {"motto": "Liberté, Égalité, Fraternité", "currency": "EUR"}}
      """


type alias Mottos =
    { countries : List Country }


type alias Country =
    { name : String
    , motto : String
    , currency : String
    }

decodeMottos : J.Decoder Mottos
decodeMottos = Debug.todo ""
```

**Date decoder**

Use `elm/time` and `rtfeldman/elm-iso8601-date-strings` (if needed):

```elm
decodeDate : J.Decoder Time.Posix
decodeDate = Debug.todo ""

> J.decodeString decodeDate "\"2018-10-01T12:48:00.000Z\""
Ok (Posix 1538398080000)
  : Result J.Error Time.Posix
```

**Account info**

Use pipeline method from `NoRedInk/elm-json-decode-pipeline` to decode this:

```elm
type alias AccountInfo  =
   { id : Int
   , email : String
   , full_name : Maybe String
   , phone_number : Maybe String
   , info_complete : Bool
   }

decodeAccountInfo : J.Decoder AccountInfo
decodeAccountInfo = Debug.todo ""

-- optional (we'll learn how to encode values in future weeks)
encodeAccountInfo : AccountInfo -> Json.Encode.Value
encodeAccountInfo val = Debug.todo ""

> J.decodeString decodeAccountInfo (Json.Encode.encode 2 (encodeAccountInfo {id=1,email="foo@bar.com",full_name=Just "Full Name",phone_number=Just "+380731267381",info_complete=False}))
Ok { email = "foo@bar.com", full_name = Just ("Full Name"), id = 1, info_complete = False, phone_number = Just "+380731267381" }
    : Result J.Error AccountInfo
> J.decodeString decodeAccountInfo "{\n  \"id\": 1,\n  \"email\": \"foo@bar.com\",\n  \"full_name\": \"Full Name\",\n  \"phone_number\": null,\n  \"info_complete\": false\n}"
Ok { email = "foo@bar.com", full_name = Just ("Full Name"), id = 1, info_complete = False, phone_number = Nothing }
    : Result J.Error AccountInfo
```

**jsonPair**

```elm
jsonPair : J.Decoder a -> J.Decoder b -> J.Decoder ( a, b )
jsonPair a b = Debug.todo ""

> J.decodeString (jsonPair J.string J.string) "[\"Louee\", \"Baton\"]"
Ok ("Louee","Baton")
    : Result J.Error ( String, String )
> J.decodeString (jsonPair J.string J.string) "[\"Louee\"]"
Err (Failure ("Expecting a list of two elements") <internals>)
    : Result J.Error ( String, String )
```

**CRUD (minimal scope)**

Implement CRUD from http://eugenkiss.github.io/7guis/tasks/ without server calls

**CRUD (full scope)**

Use [crud.hs](./crud.hs) (or make your own, similar web-server) and make actual HTTP/JSON calls. It works like this:

```
$ curl -Ss -H 'Content-Type: application/json' \
    -XPOST --data '{"name": "Valentyn", "surname": "Silvestrov"}' \
    localhost:8000/api/users/create.json
{"name":"Valentyn","id":1,"surname":"Silvestrov"}
$ curl -Ss -H 'Content-Type: application/json' \
    -XGET localhost:8000/api/users/list.json
[{"name":"Valentyn","id":1,"surname":"Silvestrov"}]
$ curl -Ss -H 'Content-Type: application/json' \
    -XPUT --data '{"name": "Valentyn Vasylyovych", "surname": "Silvestrov"}' \
    localhost:8000/api/users/1/update.json
{"name":"Valentyn Vasylyovych","id":1,"surname":"Silvestrov"}
$ curl -Ss -H 'Content-Type: application/json' \
    -XGET localhost:8000/api/users/list.json
[{"name":"Valentyn Vasylyovych","id":1,"surname":"Silvestrov"}]
$ curl -Ss -H 'Content-Type: application/json' \
    -XDELETE localhost:8000/api/users/1/delete.json
[]
$ curl -Ss -H 'Content-Type: application/json' \
    -XGET localhost:8000/api/users/list.json
[]
```

It serves current directory, so if your homework is in Homework.html,
copy `crud.hs` to it, launch and open
http://localhost:8000/ and choose Crud.html in it

Install stack via https://docs.haskellstack.org/en/stable/README/ and
just run as `./crud.hs` (should be marked as executable).

**(optional) Morse Code**

Decode the Morse Code https://www.codewars.com/kata/decode-the-morse-code/elm
