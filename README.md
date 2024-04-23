# Stracy - A linguagem dos corredores

O objetivo desta linguagem é auxiliar corredores a gerenciar seus treinos de forma mais eficiente. Atravando de variáveis que armazenam distâncias percorridas, tempos de treino e parciais de treinos, o corredor pode utilizar essas funcionalidades para gerenciar melhor seus treinos. Como a linguagem utiliza de uma funcionalidade parecida com a de um dicionário, seu acesso ainda não está representado na EBNF, mas a ideia é que o corredor possa acessar as variáveis de distância, tempo e parciais de treino de forma semelhante a um dicionário, como por exemplo: `DISTANCIA[percurso_a]` ou `TEMPO[percurso_a]`.

## EBNF

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

Esta EBNF pode ser testada no site https://jacquev6.github.io/DrawGrammar/

## Exemplo de uso da linguagem

### Exemplo 1
```stracy
DISTANCIA percurso_a PARCIAL 3 => 40;
PARCIAL percurso_a  => 17, 13, 10;
TEMPO percurso_a => 01:30:00, 01:10:00, 00:50:00;

SE (PARCIAL[percurso_a] 1:3 > DISTANCIA[percurso_a])
	:: "soma das parciais não equivale à distância total da variável percurso_a"
SENÃO
	:: "soma das parciais equivale à distância total da variável percurso_a"
```

### Exemplo 2
```stracy
DISTANCIA percurso_b PARCIAL => 20;
PARCIAL percurso_b;
TEMPO percurso_b;

contador = 0;

ENQUANTO (DISTANCIA[percurso_b] > 0)
	PARCIAL[percurso_b] contador => LEITURA
	TEMPO[percurso_b] contador => LEITURA
	
	DISTANCIA[percurso_b] => DISTANCIA[percurso_b] - PARCIAL[percurso_b] contador

	contador = contador + 1
```