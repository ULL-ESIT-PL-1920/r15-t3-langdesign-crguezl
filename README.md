# Diseño de Lenguajes (Reto)

Modifica la gramática corrigiendo los errores que veas, de manera que genere frases como estas:

1. `a[4][b+c][5] = c[2][3]`
2. `a.b.c = a[x]`
3. `a.b[x*y].d = c.d[3][1].e(temp)` 
4. à.b = f(x).z[4]

Procura que exprese bien la precedencia de operadores (vigila la asignación)

a = b[4+5]
[]


## Grammar

```ruby
<program> ::= <block>

<block> ::= '{' <statement>* '}'  // Modified Casiano

<statement> ::=
               <declaration> |
              "if" <parenthesis> <block> ("else" "if" <block>)* ('else' <block>)? |
              "while" <parenthesis> <block> |
              'function' <word> '(' <word> (',' <word>)* ')' <block> |
              <expr> ";"
              
<declaration> ::= 'var' WORD ('=' <expr>)?

<expr> = (<leftVal> '=')* <comp>

<leftVal> = WORD ('.' WORD | '[' <expr> ']')*

<comp> ::= <term> (('==', '!=', '>', '>=', '<', '<=') <term>)*

<term> ::= <sum> (('+', '-') <sum>)*

<sum> ::= <fact> (('*', '/') <fact>)*


<apply> ::= '(' <expr> (',' <expr>)* ')'<apply> | '.'<word><apply> | empty

<array> ::= '[' ']' | '[' <expr> (',' <expr> )*] // Added by Casiano

<parenthesis> ::= '(' <expr> ')'

<fact> ::=  (WORD | VALUE | <array> | <parenthesis>) ('.' WORD | '[' <expr> ']'| <apply>)*

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
