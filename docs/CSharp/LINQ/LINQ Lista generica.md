<font face="Calibri">

# ⭐ LINQ - Lista Genérica

[`◀️ voltar`](./Readme.md)
[`⬆️ inicio`](../README.md)

---

## Exemplos

```csharp
class Program
{
    static void Main(string[] args)
    {
        // Criar uma lista de pessoas
        var pessoas = new List<Pessoa>
        {
            new Pessoa { Id = 1, Nome = "Gandalf" },
            new Pessoa { Id = 2, Nome = "Bilbo" },
            new Pessoa { Id = 3, Nome = "Thorin" }
        };

        // Função para imprimir os itens de uma lista
        void PrintList<T>(IEnumerable<T> lista)
        {
            foreach (var item in lista)
            {
                Console.WriteLine(item);
            }
        }

        Console.WriteLine("Lista original:");
        PrintList(pessoas);

        // Exemplo de LINQ: Filtrar pessoas com nomes contendo "o"
        var pessoasComO = pessoas.Where(p => p.Nome.Contains("o")).ToList();

        Console.WriteLine("\nPessoas com nomes contendo 'o':");
        PrintList(pessoasComO);

        // Exemplo de LINQ: Ordenar a lista por nome
        var pessoasOrdenadas = pessoas.OrderBy(p => p.Nome).ToList();

        Console.WriteLine("\nLista ordenada por nome:");
        PrintList(pessoasOrdenadas);

        // Exemplo de LINQ: Projetar apenas os nomes das pessoas
        var nomesDasPessoas = pessoas.Select(p => p.Nome).ToList();

        Console.WriteLine("\nNomes das pessoas:");
        PrintList(nomesDasPessoas);

        // Exemplo de LINQ: Contar o número de pessoas com nomes contendo "o"
        var numeroDePessoasComO = pessoas.Count(p => p.Nome.Contains("o"));

        Console.WriteLine($"\nNúmero de pessoas com nomes contendo 'o': {numeroDePessoasComO}");

        // Exemplo de LINQ: Verificar se todas as pessoas têm nomes com mais de 3 caracteres
        var todosNomesComMaisDe3Caracteres = pessoas.All(p => p.Nome.Length > 3);

        Console.WriteLine($"\nTodos os nomes têm mais de 3 caracteres: {todosNomesComMaisDe3Caracteres}");

        // Exemplo de LINQ: Encontrar a primeira pessoa com nome "Bilbo"
        var primeiraPessoaComNomeBilbo = pessoas.FirstOrDefault(p => p.Nome == "Bilbo");

        Console.WriteLine($"\nPrimeira pessoa com nome 'Bilbo': {primeiraPessoaComNomeBilbo}");

        Console.ReadKey();
    }
}

public class Pessoa
{
    public int Id { get; set; }
    public string Nome { get; set; }

    public override string ToString()
    {
        return $"{Id} - {Nome}";
    }
}
```

---

[`^ topo`](#Dev)
</font>
