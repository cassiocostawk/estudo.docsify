<font face="Calibri">

# 🔃 Muitos para Muitos [`n:m`]

[`⬆️ inicio`](../../Readme.md)
[`◀️ voltar`](../Readme.md)

---

## Exemplo

```csharp
public class Estudante
{
    public int EstudanteId { get; set; }
    public string Nome { get; set; }
    public ICollection<EstudanteCurso> EstudanteCursos { get; set; }
}

public class Curso
{
    public int CursoId { get; set; }
    public string Nome { get; set; }
    public ICollection<EstudanteCurso> EstudanteCursos { get; set; }
}

public class EstudanteCurso
{
    public int EstudanteId { get; set; }
    public int CursoId { get; set; }
    public Estudante Estudante { get; set; }
    public Curso Curso { get; set; }
}
```

### ModelConfiguration

#### EstudanteConfiguration

```csharp
public class EstudanteConfiguration : IEntityTypeConfiguration<Estudante>
{
    public void Configure(EntityTypeBuilder<Estudante> builder)
    {
        builder.ToTable("Estudantes");

        builder.HasKey(e => e.EstudanteId);

        builder.Property(e => e.Nome)
            .IsRequired()
            .HasMaxLength(255);

        /* Relacionamentos */
        builder.HasMany(e => e.EstudanteCursos)
            .WithOne(ec => ec.Estudante)
            .HasForeignKey(ec => ec.EstudanteId);
    }
}
```

#### CursoConfiguration

```csharp
public class CursoConfiguration : IEntityTypeConfiguration<Curso>
{
    public void Configure(EntityTypeBuilder<Curso> builder)
    {
        builder.ToTable("Cursos");

        builder.HasKey(c => c.CursoId);

        builder.Property(c => c.Nome)
            .IsRequired()
            .HasMaxLength(255);

        /* Relacionamentos */
        builder.HasMany(c => c.EstudanteCursos)
            .WithOne(ec => ec.Curso)
            .HasForeignKey(ec => ec.CursoId);
    }
}
```

#### EstudanteCursoConfiguration

```csharp
public class EstudanteCursoConfiguration : IEntityTypeConfiguration<EstudanteCurso>
{
    public void Configure(EntityTypeBuilder<EstudanteCurso> builder)
    {
        builder.ToTable("EstudanteCursos");

        builder.HasKey(ec => new { ec.EstudanteId, ec.CursoId });
        
        /* Relacionamentos */
        builder.HasOne(ec => ec.Estudante)
            .WithMany(e => e.EstudanteCursos)
            .HasForeignKey(ec => ec.EstudanteId);

        builder.HasOne(ec => ec.Curso)
            .WithMany(c => c.EstudanteCursos)
            .HasForeignKey(ec => ec.CursoId);
    }
}
```

---

## Detalhes

<details><summary>Ver mais</summary>


```csharp
public class Produto
{
    public int Id { get; internal set; }
    // ...
    public IList<PromocaoProduto> Promocoes { get; set; } // <-- n
}

public class Promocao
{
    public int Id { get; set; }
    // ...
    public IList<PromocaoProduto> Produtos { get; internal set; } // <-- m
}

// Criar uma classe PromocaoProduto para representar a tabela de join entre Produto e Promocao.
public class PromocaoProduto // <-- Tabela que vai relacionar n:m
{
    public int ProdutoId { get; set; }
    public int PromocaoId { get; set; }
    public Produto Produto { get; set; }
    public Promocao Promocao { get; set; }
}

// No DbContext - no método sobrescrito OnModelCreating é necessário descrever o mapeamento da PK de PromocaoProduto
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    // A entidade PromocaoProduto tem como chave primária composta PromocaoId e ProdutoId
    modelBuilder // construtor de modelo
        .Entity<PromocaoProduto>()
        .HasKey(pp => new { pp.PromocaoId, pp.ProdutoId }); // <-- 

    base.OnModelCreating(modelBuilder);
}
```

---

### Recuperação de dados (n:m)

Exemplo:
```csharp
var promocao = context2
    .Promocoes
    .Include(p => p.Produtos)      // <--
    .ThenInclude(pp => pp.Produto) // <--
    .FirstOrDefault();

// outra forma de fazer
var lista = contexto.Promocoes.Include("Produtos.Produto");    
```

+ O método `Include` possui uma segunda sobrecarga, que permite informarmos como argumento de entrada uma string com o nome da propriedade de navegação a ser incluída no join. 
+ A vantagem dessa abordagem é que não precisamos usar outros métodos `ThenInclude` para continuar a navegação em outras entidades.
+ A desvantagem é que se o nome da propriedade mudar, teremos que lembrar todos os lugares onde fizemos isso, porque não teremos ajuda do compilador.

### Mapeamento - Usando DB Legado

+ EF Core não suporte esse tipo de cardinalidade (n:m), oque faz ser necessário ter uma classe intermediária para fazer o relacionamento (1:n) dos dois lados, conforme exemplos abaixo:

```csharp
public class Filme // chamada de classe principal
{
    // ...
    public IList<FilmeAtor> Atores { get; set; } // propriedade de navegação (do tipo coleção)

    public Filme() // <-- criar a lista no construtor
    {
        Atores = new List<FilmeAtor>();
    }

    // ...
}

public class Ator // chamada de classe principal
{
    // ...
    public IList<FilmeAtor> Filmografia { get; set; } // propriedade de navegação

    public Ator() // <-- criar a lista no construtor
    {
        Filmografia = new List<FilmeAtor>(); 
    }

    // ...
}

public class FilmeAtor // chamada de classe dependente 
{
    public Filme Filme { get; set; } // propriedade de navegação (do tipo referencia)
    public Ator Ator { get; set; } // propriedade de navegação
}
```

+ Com essa estrutura, só será necessário agora mapear os relacionamentos no `OnModelCreating`(Configuration separado),
  já que nesse caso as tabelas e colunas são diferentes da convensão do EF:

```csharp
public void Configure(EntityTypeBuilder<FilmeAtor> builder)
{
    builder.ToTable("film_actor");

    builder.Property<int>("film_id").IsRequired();
    builder.Property<int>("actor_id").IsRequired();
    builder.Property<DateTime>("last_update")
        .IsRequired()
        .HasColumnType("datetime")
        .HasDefaultValueSql("getdate()");

    builder.HasKey("film_id", "actor_id"); // shadow property

    builder // <--
        .HasOne(fa => fa.Filme)
        .WithMany(f => f.Atores)
        .HasForeignKey("film_id");

    builder // <--
        .HasOne(fa => fa.Ator)
        .WithMany(f => f.Filmografia)
        .HasForeignKey("actor_id");
}
```

</details>

---

[`^ topo`](#⭐-muitos-para-muitos-nm)
</font>
