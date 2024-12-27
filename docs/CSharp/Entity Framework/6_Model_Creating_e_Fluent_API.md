<font face="Calibri">

# ⭐ Model Creating e Fluent API

[`⬆️ inicio`](../../Readme.md)

[`◀️ voltar`](../Readme.md)

---

+ Mapeamento fora da camada de negócio.
+ Na classe do DbContext tem a sobrecarga `OnModelCreating` onde deve ser feito o mapeamento das entidades com o DB.
<br>
+ Vantagens:
  + É mais completo que fazer o mapeamento com `DataAnnotations` na Entidade.

## Implementação

Exemplo comum pelo `OnModelCreating`:

```csharp
public class AluraFilmesContexto : DbContext
{
    public DbSet<Ator> Atores { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        optionsBuilder.UseSqlServer("Data Source=(localdb)\\MSSQLLocalDB;Initial Catalog=AluraFilmes;Integrated Security=True;Connect Timeout=30;Encrypt=False;TrustServerCertificate=False;ApplicationIntent=ReadWrite;MultiSubnetFailover=False");
    }

    protected override void OnModelCreating(ModelBuilder modelBuilder) // <--
    {
        modelBuilder.Entity<Ator>()
            .ToTable("actor");

        modelBuilder.Entity<Ator>()
            .Property(a => a.Id)
            .HasColumnName("actor_id");

        modelBuilder.Entity<Ator>()
            .Property(a => a.PrimeiroNome)
            .HasColumnName("first_name")
            .HasColumnType("varchar(45)")
            .IsRequired();

        modelBuilder.Entity<Ator>()
            .Property(a => a.UltimoNome)
            .HasColumnName("last_name")
            .HasColumnType("varchar(45)")
            .IsRequired();

        modelBuilder.Entity<Ator>()
            .Property<DateTime>("last_update")
            .HasColumnType("datetime")
            .IsRequired()
            .HasDefaultValueSql("getdate()");
    }
}
```

---

### Shadow Properties

+ Shadow Property é uma coluna que não será utilizada pela aplicação mas existe no DB.

Exemplo de implementação no model creating:

```csharp
modelBuilder.Entity<Ator>()
    .Property<DateTime>("last_update")
    .HasColumnType("datetime") // explicitar o datatype
    .IsRequired() // opcional por DateTime não aceitar nulo
    .HasDefaultValueSql("getdate()"); // descrever um default através de SQL
    .HasDefaultValue(DateTime.Now); // outra alternativa
```

Setando valor em Shadow Property:

```csharp
var ator = new Ator();
ator.PrimeiroNome = "Tom";
ator.UltimoNome = "Hanks";
context.Entry(ator).Property("last_update").CurrentValue = DateTime.Now; // <--

//para atribuir:
contexto.Entry(aluno)
    .Property("nota_inicial")
    .CurrentValue = 8;
```

Obtendo valor de Shadown Property:

```csharp
string lastUpdate = context.Entry(ator).Property("last_update").CurrentValue;

//para recuperar:
var nota = contexto.Entry(aluno)
    .Property("nota_inicial")
    .CurrentValue;
```

Ordenando através de uma Shadow Property:

```csharp
var atores = context.Atores
    .OrderByDescending(a => EF.Property<DateTime>(a, "last_update")) // <--
    .Take(10);
```

#### 🔵 Em relacionamentos

> Por convenção o Entity criará shadow properties quando descobrir um relacionamento entre classes entidades mas não encontrar, na classe dependente da relação, uma propriedade que represente a chave estrangeira.

---

## Abstração para classe

+ Para que o `OnModelCreating` não fique carregado, há como abstrair essa configuração para uma classe externa.
+ Criar classe na mesma pasta do Contexto nesse formato: `EntidadeConfiguration`.
+ Essa classe deve implementar a interface `IEntityTypeConfiguration<Entidade>` que irá gerar o método `Configure`.
+ Nesse método `Configure` deve ser implementado, parecido como é feito no `modelBuilder` a `FluentAPI`, exemplo abaixo:

```csharp
public class AtorConfiguration : IEntityTypeConfiguration<Ator>
    {
        public void Configure(EntityTypeBuilder<Ator> builder)
        {
            builder.ToTable("actor");

            builder // <-- diferente do modelBuilder, só deve ser informado "builder"
                .Property(a => a.Id)
                .HasColumnName("actor_id");

            builder
                .Property(a => a.PrimeiroNome)
                .HasColumnName("first_name")
                .HasColumnType("varchar(45)")
                .IsRequired();

            builder
                .Property(a => a.UltimoNome)
                .HasColumnName("last_name")
                .HasColumnType("varchar(45)")
                .IsRequired();

            builder
                .Property<DateTime>("last_update")
                .HasColumnType("datetime")
                .IsRequired()
                .HasDefaultValueSql("getdate()");
        }
    }
```

---

### Chave Primária composta

```csharp
public class FilmeAtorConfiguration : IEntityTypeConfiguration<FilmeAtor>
{
    public void Configure(EntityTypeBuilder<FilmeAtor> builder)
    {
        builder.ToTable("film_actor");

        // shadow properties
        builder.Property<int>("film_id").IsRequired();
        builder.Property<int>("actor_id").IsRequired();
        builder.Property<DateTime>("last_update")
            .IsRequired()
            .HasColumnType("datetime")
            .HasDefaultValue("getdate()");

        builder.HasKey("film_id", "actor_id"); 
    }
}
```

---

### Foreign Key (n:m)

```csharp
public void Configure(EntityTypeBuilder<FilmeAtor> builder)
{
    // ...

    builder
        .HasOne(fa => fa.Filme)   // tem 1 propriedade Filme
        .WithMany(f => f.Atores)  // com muitos atores
        .HasForeignKey("film_id");

    builder
        .HasOne(fa => fa.Ator)          // tem 1 propriedade Ator
        .WithMany(f => f.Filmografia)   // com muitos filmes
        .HasForeignKey("actor_id");
}
```

---

### Nullable Property (nullable types)

```csharp
builder.Property<byte>("language_id");
builder.Property<byte?>("original_language_id"); // <-- <byte?>
```

---

### Indices

```csharp
builder
    .HasIndex(a => a.UltimoNome)
    .HasName("idx_actor_last_name"); 
```

*HasName é necessário somente se quiser mudar o nome padrão.*

---

### Unique (AlternateKey)

```csharp
builder
    .HasAlternateKey(a => new { a.PrimeiroNome, a.UltimoNome });
```

*Neste caso usado em 2 colunas.*

---

### Checks

>*EF não suporta restrições do tipo check, então deve ser feito através de script*.

+ Criar uma migração vazia `Add-Migration <nome>` e nela inserir o script que cria a constraint check no método `Up`, e um script que dropa ela no método `Down`:

```csharp
public partial class FilmeCheckConstraint : Migration
{
    protected override void Up(MigrationBuilder migrationBuilder)
    {
        var sql = @"ALTER TABLE [dbo].[film]
                    ADD CONSTRAINT [check_rating] CHECK (
                        [rating]='NC-17' OR 
                        [rating]='R' OR 
                        [rating]='PG-13' OR 
                        [rating]='PG' OR 
                        [rating]='G');";

        migrationBuilder.Sql(sql);
    }

    protected override void Down(MigrationBuilder migrationBuilder)
    {
        migrationBuilder.Sql(@"ALTER TABLE [dbo].[film] DROP CONSTRAINT [check_rating]");
    }
}
```

### Ignorar Propriedade

+ Para não adicionar propriedade ao DB, deve-se usar o método `Ignore`:

```csharp
builder.Ignore(f => f.Classificacao);
```

### Enumerator

+ Um Enumerador pode salvar  por padrão o índice sequencial do `Enumerator` ou ser convertido carregado em um objeto do seu tipo e convertido em uma propriedade que armazenará no DB um tipo de dados diferente.

```csharp
public class Filme
{
    // ...

    public string TextoClassificacao { get; private set; } // propriedade mapeada no contexto
    public ClassificacaoIndicativa Classificacao           // propriedade para manipular o valor, que não será mapeada para o contexto 
    {
        get { return TextoClassificacao.ParaValor(); }
        set { TextoClassificacao = value.ParaString(); }
    }

    // ...
}

public class FilmeConfiguration : IEntityTypeConfiguration<Filme>
{
    public void Configure(EntityTypeBuilder<Filme> builder)
    {
        // ...

        // propriedade mapeada no contexto
        builder
            .Property(f => f.TextoClassificacao)
            .HasColumnName("rating")
            .HasColumnType("varchar(10)");

        // propriedade para manipular o valor, que não será mapeada para o contexto 
        builder
            .Ignore(f => f.Classificacao);
    }
}
```

---

### Herança

+ Considerando que temos uma classe base chamada `Pessoa`:

```csharp
public class Pessoa
{
    public byte Id { get; set; }
    public string PrimeiroNome { get; set; }
    public string UltimoNome { get; set; }
    public string Email { get; set; }
    public bool Ativo { get; set; }

    public override string ToString()
    {
        var tipo = this.GetType().Name;
        return $"{tipo} - ({Id}) {PrimeiroNome} {UltimoNome} {Ativo}";
    }
}
```

+ E a classe `Funcionario` derivada de `Pessoa`:

```csharp
public class Funcionario : Pessoa
{
    public string Login { get; set; }
    public string Senha { get; set; }
}
```

+ O mapeamento de tais será feito da seguinte forma:
+ PessoaConfiguration:

```csharp
// PessoaConfiguration<T> - passa a ser genérica para permitir as classes derivadas
// IEntityTypeConfiguration<T> - mesmo motivo de cima
// where T : Pessoa - restringir o tipo, que tem que herdar de Pessoa
public class PessoaConfiguration<T> : IEntityTypeConfiguration<T> where T : Pessoa 
{
    // virtual para permitir - override (sobrescrita) logo que a classe derivada usará o método abaixo + o mapeamento especifico dela
    // EntityTypeBuilder<T> - genérico para funcionar nas classes derivadas
    public virtual void Configure(EntityTypeBuilder<T> builder)
    {
        builder
            .Property(c => c.PrimeiroNome)
            .HasColumnName("first_name")
            .HasColumnType("varchar(45)")
            .IsRequired();

        // ... exemplos comuns

        builder
            .Property<DateTime>("last_update")
            .HasColumnType("datetime")
            .HasDefaultValueSql("getdate()")
            .IsRequired();
    }
}
```

+ FuncionarioConfiguration:

```csharp
// herda de PessoaConfiguration para o tipo Funcionario (e não mais de IEntityTypeConfiguration<T>)
public class FuncionarioConfiguration : PessoaConfiguration<Funcionario>
{
    // override - para sobrescrever o método da classe base
    public override void Configure(EntityTypeBuilder<Funcionario> builder)
    {
        // devido a sobrescrita é necessário chamar o Configure da classe base
        base.Configure(builder); 

        // abaixo o mapeamento específico da classe derivada
        builder
            .ToTable("staff");

        builder
            .Property(f => f.Id)
            .HasColumnName("staff_id");

        builder
            .Property(f => f.Login)
            .HasColumnName("username")
            .HasColumnType("varchar(16)")
            .IsRequired();

        builder
            .Property(f => f.Senha)
            .HasColumnName("password")
            .HasColumnType("varchar(40)");
    }
}
```

*No contexto somente serão implementadas as entidades das classes derivadas.*

#### Padrões de Mapeamento de Hierarquia

+ TPH - Table per Hierarchy
  + **Único disponível no EF**
  + **Conversão do Entity**
  + Através da classe mais ancestral
    + irá criar uma tabela que icorpore todas as colunas dos seus decendentes
    + e também irá criar uma coluna pra discriminar de onde veio aquele registro
+ TPC - Table per Concrete Class
  + Cria uma tabela pra cada classe concreta
  + Como exemplo de Herança acima, porém mapeando Pessoa também
+ TPT - Table per Type
  + Cria uma tabela pra todos os tipos da hierarquia.

---

[`^ topo`](#⭐-model-creating-e-fluent-api)
</font>
