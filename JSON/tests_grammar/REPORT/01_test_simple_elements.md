

Test of parser: "string"
========================


Match-test "M1"
----------------

### Test-code:

    "string"

### AST

    (string (plain "string"))


Match-test "M2*"
-----------------

### Test-code:

    "Text with \uc4a3 unicode"

### CST

    (string (plain "Text with ") (unicode "c4a3") (plain " unicode"))

### AST

    (string (plain "Text with ") (unicode "c4a3") (plain " unicode"))


Test of parser: "number"
========================


Match-test "M1"
----------------

### Test-code:

    1

### AST

    (number "1")


Match-test "M2"
----------------

### Test-code:

    1.1

### AST

    (number "1.1")


Match-test "M3"
----------------

### Test-code:

    0

### AST

    (number "0")


Match-test "M4"
----------------

### Test-code:

    1.43E+22

### AST

    (number "1.4322")


Match-test "M5"
----------------

### Test-code:

    20

### AST

    (number "20")


Match-test "M6"
----------------

### Test-code:

    -1.3e+25

### AST

    (number "-1.325")