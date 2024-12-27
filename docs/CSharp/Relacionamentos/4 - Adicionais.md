<font face="Calibri">

# 🔃 Informações Adicionais

[`⬆️ inicio`](../../Readme.md)

[`◀️ voltar`](../Readme.md)

---

### Adicionar o Detail faz com que o Entity Framework adicione o Master

```csharp
var paoFrances = new Produto()
{
    Nome = "Pão Francês",
    PrecoUnitario = 0.40,
    Unidade = "Unidade",
    Categoria = "Padaria"
};

var compra = new Compra();
compra.Quantidade = 6;
compra.Produto = paoFrances;
compra.Preco = paoFrances.PrecoUnitario * compra.Quantidade;
using (var context = new LojaContext())
{
    context.Compras.Add(compra); // <-- está adicionando também o Produto
    context.SaveChanges();
}
```

*Ao adicionar o objeto compra no contexto, o Entity Framework irá perceber que existe uma referência não-nula para um Produto (no exemplo, o objeto caneta), e colocará esse produto sob a supervisão do ChangeTracker. Logo, o produto será inserido no banco de dados, e logo após a compra também será.*

---

### Não adicionar DbSet para um Detail sem necessidade

+ Não é necessário adicionar um `DbSet` para um `Detail`,
 quando ele está associada a outro `DbSet`, pois, dessa forma será gerada a tabela normalmente, porém não aparecerá para usar em no `DbContext.xxxxxx` oque pode ser bom para o encapsulamento de `Detail`.

### Mapeamento automático de PK

+ Id
+ EntidadeId

Chave Primária composta:

```csharp
public class FilmeAtor
{
    public int AtorId { get; set; }
    public int FilmeId { get; set; }
}
```

### Multiplos relacionamentos entre as mesmas tabelas

+ Situação em que 1 tabela se relaciona 2x com 1 outra tabela, exemplo:

```csharp
public class Filme
{
    public int Id { get; set; }
    // ...
    public Idioma IdiomaFalado { get; set; }    // <-- propriedade de navegaçao para Idioma
    public Idioma IdiomaOriginal { get; set; }  // <-- propriedade de navegaçao para Idioma

    // não cria as properties do tipo int (fk) para não sujar o código com algo que não irá utilizar em código
    // então só se mapea como shadow properties 

    // ...
}

public class Idioma
{
    public byte Id { get; set; }
    public string Nome { get; set; }
    public IList<Filme> FilmesFalados { get; set; }
    public IList<Filme> FilmesOriginais { get; set; }

    public Idioma()
    {
        FilmesFalados = new List<Filme>(); 
        FilmesOriginais = new List<Filme>();
    }

    // ...
}
```

+ Nesse caso o EF não faz o relacionamento automático, então é necessário fazê-lo manualmente:

```csharp
public class IdiomaConfiguration : IEntityTypeConfiguration<Idioma>
{
    public void Configure(EntityTypeBuilder<Idioma> builder)
    {
        builder.ToTable("language");

        builder
            .Property(i => i.Id)
            .HasColumnName("language_id");

        builder
            .Property(i => i.Nome)
            .HasColumnName("name")
            .HasColumnType("char(20)")
            .IsRequired();

        builder
            .Property<DateTime>("last_update")
            .HasColumnType("datetime")
            .HasDefaultValueSql("getdate()")
            .IsRequired();
    }   
}

public class FilmeConfiguration : IEntityTypeConfiguration<Filme>
{
    public void Configure(EntityTypeBuilder<Filme> builder)
    {
        // ...

        // shadow properties
        builder.Property<byte>("language_id");
        builder.Property<byte?>("original_language_id");

        // foreign keys
        builder
            .HasOne(f => f.IdiomaFalado)
            .WithMany(i => i.FilmesFalados)
            .HasForeignKey("language_id");

        builder
            .HasOne(f => f.IdiomaOriginal)
            .WithMany(i => i.FilmesOriginais)
            .HasForeignKey("original_language_id");
    }
} 
```

---

[`^ topo`](#⭐-informações-adicionais)
</font>
