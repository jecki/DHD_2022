

Test of parser: "array"
=======================


Match-test "M1"
----------------

### Test-code:

    [1, 2, 3]

### AST

    (array (number "1") (number "2") (number "3"))


Match-test "M2"
----------------

### Test-code:

    [1, 2, "drei"]

### AST

    (array (number "1") (number "2") (string (plain "drei")))


Match-test "M3"
----------------

### Test-code:

    [1, 3, "drei", ["vier", 5]]

### AST

    (array (number "1") (number "3") (string (plain "drei")) (array (string (plain "vier")) (number "5")))


Test of parser: "object"
========================


Match-test "M1"
----------------

### Test-code:

    { "eins": 1,
     "zwei": 2 }

### AST

    (object (member (string (plain "eins")) (number "1")) (member (string (plain "zwei")) (number "2")))


Test of parser: "json"
======================


Match-test "M1"
----------------

### Test-code:

    {
      "object":
        {
          "one": 1,
          "two": 2,
          "three": ["3"],
          "fraction": 1.5,
          "unicode": "Text with \uc4a3(unicode)"
        },
      "array": ["one", 2, 3],
      "string": " string example ",
      "true": true,
      "false": false,
      "null": null
    }

### AST

    (json
      (object
        (member
          (string
            (plain "object"))
          (object
            (member
              (string
                (plain "one"))
              (number "1"))
            (member
              (string
                (plain "two"))
              (number "2"))
            (member
              (string
                (plain "three"))
              (array
                (string
                  (plain "3"))))
            (member
              (string
                (plain "fraction"))
              (number "1.5"))
            (member
              (string
                (plain "unicode"))
              (string
                (plain "Text with ")
                (unicode "c4a3")
                (plain "(unicode)")))))
        (member
          (string
            (plain "array"))
          (array
            (string
              (plain "one"))
            (number "2")
            (number "3")))
        (member
          (string
            (plain "string"))
          (string
            (plain " string example ")))
        (member
          (string
            (plain "true"))
          (true))
        (member
          (string
            (plain "false"))
          (false))
        (member
          (string
            (plain "null"))
          (null))))


Match-test "M2"
----------------

### Test-code:

    
    
    {
        "leading and trailing whitespace": true
    }
    
    

### AST

    (json (object (member (string (plain "leading and trailing whitespace")) (true))))


Fail-test "F1"
---------------

### Test-code:
    {
            "nested object': {                    // wrong quotation mark
                "key 1" : "value 1"
                "error" : "value"
            },
            "key 2": "value 2"
        }

### Messages:

3:14: Error (1010): '`:`' expected by parser 'member', but »key 1" : "...« found instead!


Fail-test "F2"
---------------

### Test-code:
    
    
    {
        "leading and trailing whitespace": True,   // should be "true", not "True"
        "bad string": "abc\xdef",
    }
    
    

### Messages:

4:40: Error (1010): '_element' expected by parser 'member', but »True,   //...« found instead!


Fail-test "F3"
---------------

### Test-code:
    {
      "object":
        {
          "one": 1,
          "two": 2,
          "three": ["3"]     // missing comma!
          "fraction": 1.5,
          "unicode": "\xc4a3"
        },
      "array": ["one", 2, 3],
      "string": " string example ",
      "true": true,
      "false": false,
      "null": null
    }

### Messages:

7:7: Error (1010): '`}`' expected by parser 'object', but »"fraction"...« found instead!