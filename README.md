# Diseño de Lenguajes (Reto)

Modifica la gramática corrigiendo los errores que veas, de manera que genere frases como estas:

1. `a[4][b+c][5] = c[2][3]`
2. `a.b.c = a[x]`
3. `a.b[x*y].d = c.d[3][1].e(temp)` 
4. à.b = f(x).z[4]

Procura que exprese bien la precedencia de operadores (vigila la asignación)

## Grammar

```ruby
<program> ::= <block>

<block> ::= '{' (<statement> ';')* '}'  // Modified Casiano

<statement> ::=
              'var' WORD ('=' <expr>)? |
              'function' <word> '(' <word> (',' <word>)* ')' <block> |
              "if" <parenthesis> <block> ("else" "if" <block>)* ('else' <block>)? |
              "while" <parenthesis> <block> |
              <expr> ";"

<expr> = (<leftVal> '=')* <comp> // Hay que esperar a ver el '=' para saber que es un <leftVal>  y no un <comp>

<leftVal> = WORD ('.' WORD | '[' <term> ']')*

<comp> ::= <term> (('==', '!=', '>', '>=', '<', '<=') <term>)?

<term> ::= <sum> (('+', '-') <sum>)*

<sum> ::= <fact> (('*', '/') <fact>)*

<fact> ::=  (WORD | VALUE | <array> | <object> | <parenthesis>) ('.' WORD | '[' <expr> ']'| <apply>)*

<apply> ::= '(' <expr> (',' <expr>)* ')'<apply> | empty

<array> ::= '[' ']' | '[' <expr> (',' <expr> )*]

<parenthesis> ::= '(' <expr> ')'

<object> ::= '{' (WORD ':' <comp>)? (',' WORD ':' <comp>)*  '}'
```

## Tokens

```Ỳacc
WORD = [a-zA-Z_]\w*
VALUE = STRING | NUMBER
STRING = "( [^"\\]   #any character except " and escape
           | \\.     #any character before an escape
           )*"
NUMBER = ([-+]?\d+     #entero
          (\.\d+)?    #flotante
          ([eE][-+]?\d+)?  #con exponente
          )
```
