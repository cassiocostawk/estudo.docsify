# Adicionar no DbContext

[`🔼 início`](../Readme.md)

## alterar AppDbContext.cs

```csharp
public virtual DbSet<Entidade> Entidade { get; set; }

// ...

protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    // ...

    modelBuilder.ApplyConfiguration(new EntidadeModelConfiguration());

    // ...
}

// ...

public void PutDeletedFilter(ModelBuilder modelBuilder)
{
    // ...

    modelBuilder.Entity<Entidade>().HasQueryFilter(x => !x.Deletado);
    
    // ...
}
```

---

[`◀️ anterior`](./02%20ModelConfiguration.md) [`próximo ▶️`](./04%20Interfaces%20de%20Repositorio.md)