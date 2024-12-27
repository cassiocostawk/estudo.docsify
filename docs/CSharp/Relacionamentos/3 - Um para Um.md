<font face="Calibri">

# 🔃 Um para Um (1:1)

[`⬆️ inicio`](../../Readme.md)
[`◀️ voltar`](../Readme.md)

---

## Exemplo

```csharp
public class Endereco 
{
    public int Numero { get; internal set; }
    public string Logradouro { get; internal set; }
    public string Complemento { get; internal set; }
    public string Bairro { get; internal set; }
    public string Cidade { get; internal set; }
    public Cliente Cliente { get; set; } // <-- 1
}

public class Cliente
{
    public int Id { get; set; }
    public string Nome { get; internal set; }
    public Endereco EnderecoDeEntrega { get; set; } // <-- 1
}

// Não é necessário criar no DbContext um DbSet para Endereco quando Detail, ou seja relacionado 
// a outra entidade mapeada como DbSet
```

### Recuperação de dados (1:1)

Exemplo:

```csharp
var cliente = context
    .Clientes
    .Include(c => c.EnderecoDeEntrega) // <--
    .FirstOrDefault();
```

---

### Criando PK e FK manualmente

+ Para não precisar criar um ID para o Detail e seu relacionamento no modelo é possível fazer conforme abaixo:

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder
        .Entity<Endereco>()
        .Property<int>("ClienteId"); // <-- Shadow Property - so existe no Banco de Dados

    modelBuilder
        .Entity<Endereco>()
        .HasKey("ClienteId"); // <-- Cliente como PK, já que é 1:1 não precisa de um ID separado como PK

    base.OnModelCreating(modelBuilder);
}
```

### ModelConfiguration

```csharp
builder.HasOne(c => c.EnderecoDeEntrega)
        .WithOne(e => e.Cliente)
        .HasForeignKey<Endereco>(e => e.ClienteId); 
        // Adicione uma propriedade à classe Endereco
}
```

---

[`^ topo`](#⭐-um-para-um-11)
</font>
