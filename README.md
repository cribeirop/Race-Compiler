# Race-Compiler

## EBNF
λ
```
BLOCK              = { STATEMENT };
STATEMENT          = ( "λ" | ASSIGNMENT | DISTANCIA | PARCIAL | TEMPO | IMPRESSAO | ENQUANTO | SE ), "\n" ;
ASSIGNMENT         = IDENTIFIER, "=", BOOL_EXP ;
DISTANCIA          = "DISTANCIA", IDENTIFIER, "PARCIAL", ( "λ" | "=>", FACTOR), ";" ;
PARCIAL            = "PARCIAL", IDENTIFIER, ( "λ" | "=>", BOOL_EXP, {",", FACTOR}), ";" ;
TEMPO              = "TEMPO", IDENTIFIER, ( "λ" | "=>", BOOL_EXP, {",", FACTOR}), ";" ;
IMPRESSAO          = "::", " ' ", BOOL_EXP, " ' " ;
ENQUANTO           = "ENQUANTO", "(", BOOL_EXP, ")", "\n", "λ", { STATEMENT, "λ" }, "\n";
SE                 = "SE", "(", BOOL_EXP, ")", "\n", "λ", { STATEMENT, "λ" }, ( "λ" | ( "SENÃO", "\n", "λ", { STATEMENT, "λ" })), "\n" ;
BOOL_EXP           = BOOL_TERM, { "or", BOOL_TERM } ;
BOOL_TERM          = REL_EXP, { "and", REL_EXP } ;
REL_EXP            = EXPRESSION, { ( "==" | ">" | "<" ), EXPRESSION } ;
EXPRESSION         = TERM, { ( "+" | "-" | ".." ), TERM } ;
TERM 	           = FACTOR, { ( "*" | "/" ), FACTOR } ;
FACTOR             = NUMBER | STRING | IDENTIFIER | ( ( "+" | "-" | "not" ), FACTOR ) | "(", BOOL_EXP, ")" | "read", "(", ")" ;
STRING             = "''", { LETTER | DIGIT | ESPECIAL_CHARACTER }, "''";
IDENTIFIER         = LETTER, { LETTER | DIGIT | "_" } ;
NUMBER             = DIGIT, { DIGIT } ;
ESPECIAL_CHARACTER = ( "'" | "<" | "=" | "!" | "?" | "." | ";" | ":" | "-" | "..." ) ;
LETTER             = ( "a" | "..." | "z" | "A" | "..." | "Z" ) ;
DIGIT              = ( "1" | "2" | "3" | "4" | "5" | "6" | "7" | "8" | "9" | "0" ) ;
```