<font face="Calibri">

# ⭐ LINQ - Lista numérica

[`◀️ voltar`](./Readme.md)
[`⬆️ inicio`](../README.md)

---

## Exemplos

```csharp
// Criar uma lista de números inteiros para demonstração
var numeros = new List<int> { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };

// Filtrar números pares
var pares = numeros.Where(num => num % 2 == 0).ToList();

// Filtrar números ímpares
var impares = numeros.Where(num => num % 2 != 0).ToList();

// Ordenar a lista em ordem crescente
var ordenadosAscendente = numeros.OrderBy(num => num).ToList();

// Ordenar a lista em ordem decrescente
var ordenadosDescendente = numeros.OrderByDescending(num => num).ToList();

// Projetar uma nova lista com o dobro de cada número
var oDobroDosNumeros = numeros.Select(num => num * 2).ToList();

// Contar o número de elementos na lista
var contador = numeros.Count();

// Verificar se todos os números são maiores que 5
var todosMaioresQueCinco = numeros.All(num => num > 5);

// Verificar se algum número é maior que 8
var algumMaiorQueOito = numeros.Any(num => num > 8);

// Encontrar o primeiro número maior que 3
var primeiroMaiorQueTres = numeros.FirstOrDefault(num => num > 3);

// Somar todos os números na lista
var soma = numeros.Sum();

// Calcular a média dos números na lista
var media = numeros.Average();

// Encontrar o número máximo na lista
var maximo = numeros.Max();

// Encontrar o número mínimo na lista
var minimo = numeros.Min();

Console.WriteLine("Números Pares:");
Console.WriteLine(string.Join(", ", pares));

Console.WriteLine("\nNúmeros Ímpares:");
Console.WriteLine(string.Join(", ", impares));

Console.WriteLine("\nNúmeros Ordenados Crescente:");
Console.WriteLine(string.Join(", ", ordenadosAscendente));

Console.WriteLine("\nNúmeros Ordenados Decrescente:");
Console.WriteLine(string.Join(", ", ordenadosDescendente));

Console.WriteLine("\nO Dobro dos Números:");
Console.WriteLine(string.Join(", ", oDobroDosNumeros));

Console.WriteLine($"\nContagem: {contador}");
Console.WriteLine($"Todos Maiores que 5: {todosMaioresQueCinco}");
Console.WriteLine($"Algum Maior que 8: {algumMaiorQueOito}");
Console.WriteLine($"Primeiro Maior que 3: {primeiroMaiorQueTres}");
Console.WriteLine($"Soma: {soma}");
Console.WriteLine($"Média: {media}");
Console.WriteLine($"Máximo: {maximo}");
Console.WriteLine($"Mínimo: {minimo}");
```

---

[`^ topo`](#Dev)
</font>
