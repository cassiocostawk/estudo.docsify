<font face="Calibri">

# 🔡 Regex

[`◀️ voltar`](./Readme.md)
[`⬆️ inicio`](../Readme.md)

---

## 📍 Expressões Regulares
+ Regex segue a tabela `ASCII`, então quando é dito que um conjunto `[A-z]` será considerada toda a sequência `decimal` de `A` a `z` ou seja do `decimal` 65 até 122, ou sejá, isso incluirá caracteres especiais de `decimal` 91 a 96, que pode confundir e gerar um erro, ou seja não usar `[A-z]` e se atentar a sequencia.

| Caractere | Decimal |   | Caractere | Decimal |   | Caractere | Decimal |
|:---------:|:-------:|:-:|:---------:|:-------:|:-:|:---------:|:-------:|
|   Espaço  |    32   |   |   **A**   |  **65** |   |   **a**   |  **97** |
|     !     |    33   |   |   **B**   |  **66** |   |   **b**   |  **98** |
|     "     |    34   |   |   **C**   |  **67** |   |   **c**   |  **99** |
|     #     |    35   |   |   **D**   |  **68** |   |   **d**   | **100** |
|     $     |    36   |   |   **E**   |  **69** |   |   **e**   | **101** |
|     %     |    37   |   |   **F**   |  **70** |   |   **f**   | **102** |
|     &     |    38   |   |   **G**   |  **71** |   |   **g**   | **103** |
|     '     |    39   |   |   **H**   |  **72** |   |   **h**   | **104** |
|     (     |    40   |   |   **I**   |  **73** |   |   **i**   | **105** |
|     )     |    41   |   |   **J**   |  **74** |   |   **j**   | **106** |
|     *     |    42   |   |   **K**   |  **75** |   |   **k**   | **107** |
|     +     |    43   |   |   **L**   |  **76** |   |   **l**   | **108** |
|     ,     |    44   |   |   **M**   |  **77** |   |   **m**   | **109** |
|     -     |    45   |   |   **N**   |  **78** |   |   **n**   | **110** |
|     .     |    46   |   |   **O**   |  **79** |   |   **o**   | **111** |
|     /     |    47   |   |   **P**   |  **80** |   |   **p**   | **112** |
|   **0**   |  **48** |   |   **Q**   |  **81** |   |   **q**   | **113** |
|   **1**   |  **49** |   |   **R**   |  **82** |   |   **r**   | **114** |
|   **2**   |  **50** |   |   **S**   |  **83** |   |   **s**   | **115** |
|   **3**   |  **51** |   |   **T**   |  **84** |   |   **t**   | **116** |
|   **4**   |  **52** |   |   **U**   |  **85** |   |   **u**   | **117** |
|   **5**   |  **53** |   |   **V**   |  **86** |   |   **v**   | **118** |
|   **6**   |  **54** |   |   **W**   |  **87** |   |   **w**   | **119** |
|   **7**   |  **55** |   |   **X**   |  **88** |   |   **x**   | **120** |
|   **8**   |  **56** |   |   **Y**   |  **89** |   |   **y**   | **121** |
|   **9**   |  **57** |   |   **Z**   |  **90** |   |   **z**   | **122** |
|     :     |    58   |   |     [     |    91   |   |     {     |   123   |
|     ;     |    59   |   |     \     |    92   |   |     \|    |   124   |
|     <     |    60   |   |     ]     |    93   |   |     }     |   125   |
|     =     |    61   |   |     ^     |    94   |   |     ~     |   126   |
|     >     |    62   |   |     _     |    95   |   |   DELETE  |   127   |
|     ?     |    63   |   |     `     |    96   |   |           |         |
|     @     |    64   |   |           |         |   |           |         |

+ Conjunto de caracteres:
  + `[0-9]`, `[a-z]`, `[A-H]`...
+ Caractere opcional:
  + `?`
  + Uso:
    + `[a-z]?`
    + pode ou não ter um caractere de `a` até `z`
+ Exceção de caractere:
  + `^`
  + Uso:
    + `[^a-z]`
    + não faz parte do intervalo de `a` até `z`

---
## 📍 Métodos C#
### IsMatch()
+ Retorna `true` se o `input` match com a expressão `pattern`.
```csharp
string exemplo1 = "Me ligue em 7899-1234";
string exemplo2 = "Meu número de telefone é 97899-1234";
string exemplo3 = "O número 78991234 é pra contato profissional";

string expressao = @"[0-9]{4,5}-?[0-9]{4}";

Console.WriteLine(Regex.IsMatch(exemplo1, expressao)); // true
Console.WriteLine(Regex.IsMatch(exemplo2, expressao)); // true
Console.WriteLine(Regex.IsMatch(exemplo3, expressao)); // true
```

---
### Match()
+ Retorna objeto do tipo `Match` que pode serinterpretado diretamente como uma string
+ `Math` tb tem seus métodos:
  + `NextMatch()` que obtém o próximo `Match` (recursivo)
```csharp
string exemplo1 = "Me ligue em 7899-1234";
string exemplo2 = "Meu número de telefone é 97899-1234";
string exemplo3 = "O número 78991234 é pra contato profissional";

string expressao = @"[0-9]{4,5}-?[0-9]{4}";

Console.WriteLine(Regex.Match(exemplo1, expressao)); // 7899-1234
Console.WriteLine(Regex.Match(exemplo2, expressao)); // 97899-1234
Console.WriteLine(Regex.Match(exemplo3, expressao)); // 78991234
```

---

[`^ topo`](#🔡-regex)
</font>
