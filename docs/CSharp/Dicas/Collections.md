<font face="Calibri">

# ⭐ Collections

[`⬆️ inicio`](../../Readme.md)

[`◀️ voltar`](../Readme.md)

---

## 📌 List

### AddRange()

+ Por mais se seja só `int` no range é preciso passar uma coleção:

```csharp
List<int> idades = new List<int>();
idades.Add(1);
idades.Add(2);
idades.AddRange(new int[] { 4, 5, 6, 7, 9 });
```

### Sort()

+ O método Sort funciona quando temos uma lista de `int`, `string` e outros tipos do .Net.
+ Para outros tipos de objetos é necessário implementar o `IComparable` no objeto ou usar o `IComparer<T>`.
+ O `Sort` ordena a própria lista, e é um método `void`, diferente do `OrderBy` que retorna um `IOrderedEnumerable` com tal lista ordenada.

#### IComparable.CompareTo()

+ Implementando a interface `IComparable` na classe `ContaCorrente` e implementando o método `CompareTo()`:

```csharp
public class ContaCorrente : IComparable
{
    public int Numero { get; }
    // ...

    public int CompareTo(object obj)
    {
        // Retornar negativo quando a instância precede o obj
        // Retornar zero quando nossa instância e obj forem equivalentes
        // Retornar positivo quando a instância precedência for de obj

        var outraConta = obj as ContaCorrente;

        // se a conversao para ContaCorrente deu errado fica nulo, então instância precede o obj
        if (outraConta == null) return -1; 

        // comparações
        if (Numero < outraConta.Numero) return -1;
        if (Numero == outraConta.Numero) return 0;
        return 1;
    }
}

// Uso
contas.Sort(); // Implementado IComparable
```

#### IComparer.Compare()

+ Implementando a interface `IComparer` numa nova classe de comparação chamada `ContaCorrenteComparer...` e implementando o método `Compare()`:

```csharp
class ContaCorrenteComparer_Agencia : IComparer<ContaCorrente>
{
    public int Compare(ContaCorrente x, ContaCorrente y) // alterado de object para o tipo
    {
        //os 2 são nulos ou apontam pro mesmo objeto
        if (x == y) return 0; 

        // um dos 2 são nulos
        if (x == null) return 1;
        if (y == null) return -1;

        // comparar
        if (x.Agencia < y.Agencia) return -1;
        if (x.Agencia == y.Agencia) return 0;
        return 1;
        // ou simplificado       
        return x.Agencia.CompareTo(y.Agencia); // ¹
    }
}

// Uso
contas.Sort(new ContaCorrenteComparer_Agencia());
```

**¹** Tipos primitivos ja implementam o `IComparable`, então dentro do objeto pode ser simplificado.

### OrderBy()

+ Para o `List`, o `OrderBy` é um **método de extensão** em `System.Linq` pois o `List` implementa o `IEnumerable`.
+ `OrderBy` retorna um `IOrderedEnumerable` que seria a lista ordenada, diferente do `Sort` que ordena a própria lista, e é um método `void`.

<details>
<summary>Ordenando um <code>List</code> de <b>Tipos Primitivos</b> <code>(int, string...)</code></summary>

```csharp
var meses = new List<string>() { "janeiro", "fevereiro", "março", "abril", "maio", "junho", "julho", 
    "agosto", "setembro", "outubro", "novembro", "dezembro" };

var mesesOrdenados = meses.OrderBy(mes => mes); // <--
```

<hr style="height:1px;border-width:0;color:#27292c;background-color:#27292c">
</details>

<details>
<summary>Ordenando um <code>List</code> por uma <b>Propriedade</b> de uma <b>Classe</b></summary>

```csharp
var contas = new List<ContaCorrente>()
{
    new ContaCorrente(123, 12349),
    new ContaCorrente(125, 12345),
    new ContaCorrente(121, 12347)
};

IOrderedEnumerable<ContaCorrente> contasOrdenadas = contas.OrderBy(c => c.Numero); // Exp. Lambda - pra esse c, selecionar c.Numero
// ou Exemplificando a expressão acima e adicionando condicao nula
IOrderedEnumerable<ContaCorrente> contasOrdenadas2 = contas.OrderBy(c =>  
    {
        if (c == null) return int.MaxValue; // para que o nulo fique no final da ordenação

        return c.Numero;
    }); 
// ou expressão acima simplificada
IOrderedEnumerable<ContaCorrente> contasOrdenadas = contas.OrderBy(c => c != null ? c.Numero : int.MaxValue);

```

+ `c` passa a ser o argumento que o `OrderBy` passou, que nesse caso é `contas`.

<hr style="height:1px;border-width:0;color:#27292c;background-color:#27292c">
</details>

### Where()

+ Para o `List`, o `Where` é um **método de extensão** em `System.Linq` pois o `List` implementa o `IEnumerable`.
+ `Where` retorna um `IEnumerable`.

<details>
<summary><b>Filtrando</b> e <b>Ordenando</b> um <code>List</code></summary>

```csharp
var contas = new List<ContaCorrente>()
{
    new ContaCorrente(123, 12349),
    null,
    new ContaCorrente(125, 12345),
    null,
    new ContaCorrente(121, 12347)
};

// uso do Where e OrderBy encadeados ¹
IEnumerable<ContaCorrente> contasResultado = contas
        .Where(c => c != null)   // Exp. Lambda - pra c onde c não é nulo
        .OrderBy(c => c.Numero); // Exp. Lambda - pra c, selecionar c.Numero

foreach (var item in contasResultado)
{
    Console.WriteLine(item);
}
```

+ ¹ Métodos **Encadeados**.

<hr style="height:1px;border-width:0;color:#27292c;background-color:#27292c">
</details>

### Inicializadores de coleção

+ Os inicializadores de objeto permitem atribuir valores a quaisquer campos ou propriedades acessíveis de um objeto na hora de criação sem que seja necessário invocar um construtor seguido por linhas de instruções de atribuição.

+ Exemplo:

```csharp
var idades = new List<int>(){ 22, 18, 27, 44 };
```

---

## 📌 Enumerable


---

[`^ topo`](#⭐-collections)
</font>
