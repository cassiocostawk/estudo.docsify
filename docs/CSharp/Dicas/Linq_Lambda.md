<font face="Calibri">

# ⭐ LINQ e Lambda

[`⬆️ inicio`](../../Readme.md)

[`◀️ voltar`](../Readme.md)

---

## LINQ

`Language Integrated Query`

+ `LINQ`, nome da tecnologia da biblioteca criada pela Microsoft, para utilizarmos essas filtros, ordenações e outros, por meio de métodos de extensão.
+ Fisicamente `LINQ` é o `namespace` no qual encontramos os operadores `Where()` e `OrderBy()` e outros.

É um componente do Microsoft .NET que adiciona funcionalidades de consulta em algumas linguagens de programação .NET. O `LINQ` corresponde a uma sintaxe unificada, inicialmente incorporada às linguagens C# e Visual Basic, para consultas em fontes de dados variadas.

+ Exemplo de método `Where` do `Linq` com `Expressão Lambda`:

```csharp
customers.Where(c => c.City == "London");
```

## Expressões Lambda

Você usa uma **expressão lambda** para criar uma **função anônima**. Use o **operador de declaração lambda** `=>` para separar a lista de parâmetros de lambda do corpo.

Uma expressão lambda pode ser de qualquer uma das duas formas a seguir:

+ Expressão lambda que tem uma expressão como corpo:

```csharp
(input-parameters) => expression
Instrução lambda que tem um bloco de instrução como corpo:
```

+ Instrução lambda que tem um bloco de instrução como corpo:

```csharp
Action<string> greet = name =>
{
    string greeting = $"Hello {name}!";
    Console.WriteLine(greeting);
};
greet("World"); // Hello World!
```

Para criar uma expressão lambda, especifique os parâmetros de entrada (se houver) no lado esquerdo do operador lambda e uma expressão ou um bloco de instrução do outro lado.

+ Dois ou mais parâmetros de entrada são separados por vírgulas:

```csharp
// delegate
Func<int, int, bool> testForEquality = (x, y) => x == y;
```

+ Outros exemplos:

```csharp
x => x.Nome
```

+ Exemplo de `Expressão Lambda` num `OrderBy`:

```csharp
// Exp. Lambda - pra c, selecionar c.Numero
contas.OrderBy(c => c.Numero); 

// ou Exemplificando a expressão 
contas.OrderBy(c => { return c.Numero; }); 

// ou/e Considerando nulo
contas.OrderBy(c =>  
    {
        if (c == null) return int.MaxValue; // para que o nulo fique no final da ordenação

        return c.Numero;
    }); 
// ou 
contas.OrderBy(c => c != null ? c.Numero : int.MaxValue);

```

+ `c` passa a ser o argumento que o `OrderBy` passou, que nesse caso é `contas`.

---

[`^ topo`](#⭐-linq-e-lambda)
</font>
