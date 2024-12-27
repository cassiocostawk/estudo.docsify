<font face="Calibri">

# ⭐ Extension Methods

[`⬆️ inicio`](../../Readme.md)

[`◀️ voltar`](../Readme.md)

---

`Métodos de Extensão`

Os `métodos de extensão` permitem que você "adicione" tipos existentes sem criar um novo tipo derivado, recompilar ou, caso contrário, modificar o tipo original.

Os métodos de extensão são **métodos estáticos**, mas são chamados como se fossem métodos de instância no tipo estendido.

> Os `métodos de extensão` mais comuns são os operadores de consulta padrão do `LINQ` que adicionam a funcionalidade de consulta aos tipos existentes `System.Collections.IEnumerable` e `System.Collections.Generic.IEnumerable<T>`. 
[Veja mais](Linq_Lambda.md)


---

## 💡 Como criar

+ Defina uma classe estática para conter o método de extensão.
+ Implemente o método de extensão como um método estático com, pelo menos, a mesma visibilidade da classe que a contém.
+ O **primeiro parâmetro** do método especifica o **tipo** no qual o método opera. Ele deve ser precedido pelo modificador `this`.
  `public static void AddLista(this List<int> lista, ...)`
+ No código de chamada, adicione uma diretiva using para especificar o using que contém a classe do método de extensão.
+ Chame os métodos como se fossem métodos de instância no tipo.
  `var lista = new List<int>().AddLista(1, 2, 3, 4);`
  *Observe que o primeiro parâmetro não é especificado pelo código de chamada*

---

## 👣 Exemplos

+ Extendendo uma `string`:

```csharp
namespace CustomExtensions
{
    public static class StringExtension // classe estática
    {
        public static int WordCount(this string str) // primeiro parametro com o modificador this
        {
            return str.Split(new char[] {' ', '.','?'}, StringSplitOptions.RemoveEmptyEntries).Length;
        }
    }
}

// Usando o método de extensão
using CustomExtensions; // Classe de extensão deve ser declarada na Using
// ...

static void Main(string[] args)
{
    string s = "The quick brown fox jumped over the lazy dogs";
    int i = s.WordCount(); // <--
    System.Console.WriteLine($"Word count of s is {i}"); 
}
```

+ Extendendo Tipos Genéricos, exemplo do `List<T>`:

```csharp
public static class ListExtension // ¹
{
    public static void AddLista<T>(this List<T> lista, params T[] itens) // ³
    {
        foreach (T item in itens)
        {
            lista.Add(item);
        }
    }
}

// Usando o método de extensão
List<int> idades = new List<int>();
            
// posso usar das 2 formas, sendo a primeira, a forma que o compilador faz pro 2º caso
ListExtension.AddLista(idades, 1, 45, 60, 70);
idades.AddLista(1, 45, 60, 70); // ² 
```

+ ¹ Não usa `ListExtension<T>` porque classe de método de extensão so pode ser estático e não genérico, então nesse caso se usa o `T` na classe, mas sim na frente do nome do método `AddLista<T>`.
+ ² Por mais que o método seja `AddList<T>`, não é preciso informar o `T` ao usar, porque o compilador consegue inferir tipos genéricos, baseando-se no `T` do `List` passado como argumento ³.

##### 🔍 Mais exemplos Extendendo o `List<T>`:

<details>
<summary>Ver mais</summary>

+ Retornar a Lista em string linha a linha:

```csharp
public static string ImprimeLista<T>(this List<T> lista)
{
    var result = "";

    foreach (var item in lista)
    {
        result += $"{item} \n";
    }

    return result;
}
```

</details>

---

[`^ topo`](#⭐-extension-methods)
</font>
