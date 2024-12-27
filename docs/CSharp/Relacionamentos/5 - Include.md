<font face="Calibri">

# ⤵️ Include

[`◀️ voltar`](./Readme.md)
[`⬆️ inicio`](../Readme.md)

---

## Entity Framework Core (EF Core)

O EF Core é uma ferramenta de mapeamento objeto-relacional (ORM) que permite trabalhar com bancos de dados relacionais usando objetos no código C#.

Aqui está a analogia:

### Tabelas SQL
Imagine que você tem três tabelas em um banco de dados SQL: `Orders`, `OrderDetails`, e `Products`. `Orders` armazena informações sobre pedidos, `OrderDetails` contém detalhes específicos de cada pedido e `Products` contém informações sobre os produtos que podem ser incluídos em um pedido.

```sql
CREATE TABLE Orders (
    OrderID INT PRIMARY KEY,
    CustomerName VARCHAR(255),
    OrderDate DATE
);

CREATE TABLE OrderDetails (
    OrderDetailID INT PRIMARY KEY,
    OrderID INT,
    ProductID INT,
    Quantity INT
);

CREATE TABLE Products (
    ProductID INT PRIMARY KEY,
    ProductName VARCHAR(255),
    Price DECIMAL(10, 2)
);
```

### Entity Framework Core

Agora, considere que você tem classes C# que mapeiam essas tabelas:

```csharp
public class Order
{
    public int OrderID { get; set; }
    public string CustomerName { get; set; }
    public DateTime OrderDate { get; set; }
    public List<OrderDetail> OrderDetails { get; set; }
}

public class OrderDetail
{
    public int OrderDetailID { get; set; }
    public int OrderID { get; set; }
    public int ProductID { get; set; }
    public int Quantity { get; set; }
    public Product Product { get; set; }
}

public class Product
{
    public int ProductID { get; set; }
    public string ProductName { get; set; }
    public decimal Price { get; set; }
}
```

Agora, vejamos como você pode usar `Include` e `ThenInclude` para recuperar dados relacionados:

**1. Include:**
`Include` é usado para carregar dados relacionados imediatos (equivalente a um `JOIN` em SQL). Por exemplo, para recuperar os detalhes de um pedido com os produtos relacionados, você faria o seguinte:

```csharp
var orderWithDetails = dbContext.Orders
    .Include(o => o.OrderDetails)
    .FirstOrDefault();
```

Isso carregará os detalhes do pedido relacionados ao pedido. O SQL gerado por EF Core terá algo semelhante a um `INNER JOIN` entre `Orders` e `OrderDetails`.

**2. ThenInclude:**
`ThenInclude` é usado quando você precisa carregar dados relacionados de níveis mais profundos. No nosso exemplo, para carregar os produtos relacionados aos detalhes do pedido, você faria o seguinte:

```csharp
var orderWithDetailsAndProducts = dbContext.Orders
    .Include(o => o.OrderDetails)
        .ThenInclude(od => od.Product)
    .FirstOrDefault();
```

Aqui, usamos `ThenInclude` para acessar a propriedade `Product` dentro de `OrderDetails`. O SQL gerado ainda realizará um `INNER JOIN` adicional entre `OrderDetails` e `Products`.

Essa é uma maneira eficaz de recuperar e carregar dados relacionados em EF Core, tornando mais fácil trabalhar com objetos C# que correspondem às tabelas SQL.

---

[`^ topo`](#Dev)
</font>
