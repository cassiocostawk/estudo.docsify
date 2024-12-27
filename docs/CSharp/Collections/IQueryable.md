<font face="Calibri">

# ⭐ IQueryable

[`◀️ voltar`](../Readme.md)
[`⬆️ inicio`](../../README.md)

---

## `IQueryable` usando expressões lambda

### Método `Where`

O método `Where` é usado para filtrar elementos com base em uma condição. Com `IQueryable`, é crucial usar expressões lambda que possam ser traduzidas para SQL. Alguns exemplos:

```csharp
// Filtrar por igualdade
IQueryable<T> query = dbContext.Entities.Where(e => e.Property == value);

// Filtrar por intervalo
IQueryable<T> query = dbContext.Entities.Where(e => e.Age >= minAge && e.Age <= maxAge);

// Combinação de condições
IQueryable<T> query = dbContext.Entities.Where(e => e.Property == value && e.IsActive);

// Uso de métodos externos
IQueryable<T> query = dbContext.Entities.Where(e => IsMatch(e.Property));
```

#### Mais além

É importante destacar que nem todas as expressões lambda podem ser traduzidas diretamente para SQL quando se trabalha com `IQueryable` e `Where`. 

Algumas operações são suportadas, enquanto outras podem resultar em exceções ou em consultas ineficientes.

Aqui estão alguns padrões comuns que podem ser traduzidos para SQL:

```csharp
// Igualdade
IQueryable<T> query1 = dbContext.Entities.Where(e => e.Property == value);

// Desigualdade
IQueryable<T> query2 = dbContext.Entities.Where(e => e.Property != value);

// Comparação numérica
IQueryable<T> query3 = dbContext.Entities.Where(e => e.Age > minValue && e.Age < maxValue);

// Operadores lógicos
IQueryable<T> query4 = dbContext.Entities.Where(e => e.IsActive && (e.Age > 21 || e.Name.StartsWith("A")));

// Verificação de pertencimento em uma lista
List<int> ids = new List<int> { 1, 2, 3 };
IQueryable<T> query5 = dbContext.Entities.Where(e => ids.Contains(e.ID));

// Combinação de condições
IQueryable<T> query6 = dbContext.Entities.Where(e => e.Property == value && e.IsActive);

// Verificação de nulidade
IQueryable<T> query7 = dbContext.Entities.Where(e => e.Property != null);

// Uso de métodos externos
IQueryable<T> query8 = dbContext.Entities.Where(e => IsMatch(e.Property));
```

Estas são apenas algumas expressões lambda comuns, e a lista pode se estender dependendo do provedor de banco de dados que você está usando (como o Entity Framework). Recomenda-se verificar a documentação específica do seu provedor de banco de dados para obter informações detalhadas sobre o suporte de tradução para SQL.

---

### Métodos `FirstOrDefault` e `SingleOrDefault`

Esses métodos retornam o primeiro (ou único) elemento que atende à condição ou o valor padrão se nenhum elemento for encontrado.

```csharp
// Obter o primeiro elemento que atende à condição
T result = dbContext.Entities.FirstOrDefault(e => e.Property == value);

// Obter o único elemento que atende à condição
T result = dbContext.Entities.SingleOrDefault(e => e.ID == targetID);
```

### Método `OrderBy`

O método `OrderBy` é usado para ordenar os elementos com base em uma chave.

```csharp
// Ordenar por uma propriedade ascendente
IQueryable<T> query = dbContext.Entities.OrderBy(e => e.Property);

// Ordenar por uma propriedade descendente
IQueryable<T> query = dbContext.Entities.OrderByDescending(e => e.Property);
```

---

### Método `Select`

O método `Select` é usado para projetar os elementos em uma forma diferente.

```csharp
// Projetar apenas algumas propriedades
IQueryable<DTO> query = dbContext.Entities.Select(e => new DTO { Prop1 = e.Prop1, Prop2 = e.Prop2 });

// Realizar cálculos durante a projeção
IQueryable<string> query = dbContext.Entities.Select(e => $"{e.FirstName} {e.LastName}");
```

Estas são apenas algumas possibilidades, e a versatilidade do LINQ permite criar consultas complexas e expressivas. Certifique-se de consultar a documentação do LINQ e do seu provedor de banco de dados para entender completamente as capacidades e limitações do LINQ to Entities.

---

[`^ topo`](#Dev)
</font>
