<font face="Calibri">

# ⭐ Entity Framework

[`⬆️ inicio`](../../Readme.md)

[`◀️ voltar`](../Readme.md)

---

## Configuração Inicial

+ Instalar o `Microsoft.EntityFrameworkCore.SqlServer`. (ou DB desejado)
+ Criar um contexto próprio que herda da classe `DbContext`. 
  + A Classe que herda do DbContext é como um DataModule.
+ Criar propriedades `DbSet` para as Entidades do Contexto para dizer quais classes serão persistidas.
  + `DbSet` são como os `Datasets` disponíveis no `DbContext`.
+ Sobrepor método `OnConfiguring` do `DbContext` na classe derivada.
  + Serve pra informar no evento de configuração do contexto o nome do banco e sua localização.

Exemplo:

```csharp
public class LojaContext : DbContext
{
    public DbSet<Produto> Produtos { get; set; }

    public LojaContext(DbContextOptions<LojaContext> options) : base(options)
    {
    }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        if (!optionsBuilder.IsConfigured) // só configura se ainda não estiver configurado
            optionsBuilder.UseSqlServer("Server=(localdb)\\mssqllocaldb;Database=LojaDB;Trusted_Connection=true;");
    }
}
```

### Inclusão

Exemplo de Inclusão:

```csharp
var contexto = new LojaContext();

Produto p1 = new Produto();
p1.Nome = "Harry Potter e a Ordem da Fênix";
p1.Categoria = "Livros";
p1.Preco = 19.89;

Produto p2 = new Produto();
p2.Nome = "Senhor dos Anéis 1";
p2.Categoria = "Livros";
p2.Preco = 19.89;

Produto p3 = new Produto();
p3.Nome = "O Monge e o Executivo";
p3.Categoria = "Livros";
p3.Preco = 19.89;

contexto.Produtos.Add(p1);
contexto.Produtos.Add(p2);
contexto.Produtos.Add(p3);

//ou

// pode trazer uma melhora significativa de performance para inclusão de muitos objetos.
contexto.Produtos.AddRange(p1, p2, p3); 

contexto.SaveChanges();
```

### Buscar

Exemplo:

```csharp
var repo = new LojaContext()
Produto produto = repo.Produtos.FirstOrDefault(); // primeiro SELECT TOP 1 * FROM ...
```

### Buscar Lista

Exemplo:

```csharp
var repo = new LojaContext();
IList<Produto> produtos = repo.Produtos.ToList();
```

### Excluir

Exemplo:

```csharp
var repo = new LojaContext();
IList<Produto> produtos = repo.Produtos.ToList();
foreach (var item in produtos)
{
    repo.Produtos.Remove(item);
}

repo.SaveChanges();
```

### Atualizar

Exemplo:

```csharp
var repo = new LojaContext()
Produto produto = repo.Produtos.FirstOrDefault();

produto.Nome = "O Senhor dos Aneis";

repo.Produtos.Update(produto);
repo.SaveChanges();
```

### Exemplo de Classe DAO com o Entity

IProdutoDAO:

```csharp
interface IProdutoDAO
{
    void Adicionar(Produto p);
    void Atualizar(Produto p);
    void Remover(Produto p);
    IList<Produto> Produtos();
}
```

ProdutoDAOEntity:

```csharp
class ProdutoDAOEntity : IProdutoDAO, IDisposable
{
    private LojaContext context;

    public ProdutoDAOEntity() // construtor para instanciar o contexto
    {
        this.context = new LojaContext();
    }

    public void Adicionar(Produto p)
    {
        context.Produtos.Add(p);
        context.SaveChanges();
    }

    public void Atualizar(Produto p)
    {
        context.Produtos.Update(p);
        context.SaveChanges();
    }

    public void Dispose()
    {
        context.Dispose();
    }

    public IList<Produto> Produtos()
    {
        return context.Produtos.ToList();
    }

    public void Remover(Produto p)
    {
        context.Produtos.Remove(p);
        context.SaveChanges();
    }
}
```

### Transações

+ Você deve estar se perguntando se o Entity suporta transações.
Por padrão, desde a versão 6, quando o Entity executa o método `SaveChanges` uma transação é criada para englobar os comandos SQL necessários para salvar as alterações do contexto no banco.

+ Se você precisar de um maior controle sobre transações em sua aplicação, poderá usar os métodos `BeginTransaction` e `UseTransaction`, disponíveis na propriedade `Database` da classe `DbContext`.

---

[`^ topo`](#⭐-entity-framework)
</font>

