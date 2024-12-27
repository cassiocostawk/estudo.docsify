<font face="Calibri">

# 🔃 Um pra Muitos [`1:n`]

[`⬆️ inicio`](../../Readme.md)
[`◀️ voltar`](../Readme.md)

---

## 📃 Exemplos

### Relacionamento Unidirecional

[`Pessoa > Cidade`]

```csharp
public class Pessoa
{
    public Guid Id { get; set; }
    public string Nome { get; set; }
    public Guid CidadeId { get; set; }
    public Cidade Cidade { get; set; }
}

public class Cidade
{
    public Guid Id { get; set; }
    public string Nome { get; set; }
}
```

#### ModelConfiguration

```csharp
public class PessoaConfiguration : IEntityTypeConfiguration<Pessoa>
{
    public void Configure(EntityTypeBuilder<Pessoa> builder)
    {
        builder.ToTable("Pessoas");

        builder.HasKey(p => p.Id);

        builder.Property(p => p.Nome)
            .IsRequired()
            .HasMaxLength(255);

        /* Relacionamento */
        builder.HasOne(p => p.Cidade)
            .WithMany()
            .HasForeignKey(p => p.CidadeId);
    }
}

public class CidadeConfiguration : IEntityTypeConfiguration<Cidade>
{
    public void Configure(EntityTypeBuilder<Cidade> builder)
    {
        builder.ToTable("Cidades");

        builder.HasKey(c => c.Id);

        builder.Property(c => c.Nome)
            .IsRequired()
            .HasMaxLength(255);

        /* NÃO HÁ relacionamento deste lado */
    }
}
```

---

### Relacionamento Bidirecional

[`Pedido <> PedidoItem`]

```csharp
public class Pedido
{
    public Guid Id { get; set; }
    public ICollection<PedidoItem> PedidoItems { get; set; }
}

public class PedidoItem
{
    public Guid Id { get; set; }
    public Guid PedidoId { get; set; }
    public Pedido Pedido { get; set; }
}
```

#### ModelConfiguration

```csharp
public class PedidoConfiguration : IEntityTypeConfiguration<Pedido>
{
    public void Configure(EntityTypeBuilder<Pedido> builder)
    {
        builder.ToTable("Pedidos");

        builder.HasKey(p => p.Id);

        /* Relacionamento */
        builder.HasMany(p => p.PedidoItems)
            .WithOne(pi => pi.Pedido)
            .HasForeignKey(pi => pi.PedidoId);
    }
}

public class PedidoItemConfiguration : IEntityTypeConfiguration<PedidoItem>
{
    public void Configure(EntityTypeBuilder<PedidoItem> builder)
    {
        builder.ToTable("PedidoItens");

        builder.HasKey(pi => pi.Id);

        /* Relacionamento */
        builder.HasOne(pi => pi.Pedido)
            .WithMany(p => p.PedidoItems)
            .HasForeignKey(pi => pi.PedidoId);
    }
}
```


---

[`^ topo`](#⭐-um-pra-muitos-1n)
</font>
