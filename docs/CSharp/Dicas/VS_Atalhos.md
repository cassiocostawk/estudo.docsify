# Visual Studio - Atalhos

### Finalizar bloco de código ( Shift + Enter )

```csharp
// antes
builder
    .Property<DateTime>("last_update")
    .HasColumnType("datetime")
    .HasDefaultValueSql("getdate()" // <-- cursor

// depois
builder
    .Property<DateTime>("last_update")
    .HasColumnType("datetime")
    .HasDefaultValueSql("getdate()"); // <-- adicionou );
```

### Cria uma linha acima e posiciona nela ( Ctrl + Enter )

```csharp
// antes
builder
    .Property<DateTime>("last_update")
    .HasColumnType("datetime")
    .HasDefaultValueSql("getdate()")
    .IsRequired(); // <-- cursor

// depois
builder
    .Property<DateTime>("last_update")
    .HasColumnType("datetime")
    .HasDefaultValueSql("getdate()")
    // <-- cursor
    .IsRequired(); 
```

### Insere Próximo Sinal de Interpolação correspondente ( Shift + Alt + . )

+ Selecione um texto e use o atalho para selecionar a próxima correspondência pra esse texto.

Simulando com [] a seleção dentro do código, veja o exemplo:
```csharp
builder
    .Property(c => c.UltimoNome)
    .[HasColumn]Name("last_name")     // Primeiro texto selecionado "HasColumn"
    .[HasColumn]Type("varchar(45)")   // Após usar atalho 1x seleciona também este
    .IsRequired();

builder
    .Property(c => c.Email)
    .[HasColumn]Name("email")        // Após usar atalho 2x seleciona este também
    .[HasColumn]Type("varchar(45)"); // Após usar atalho 3x seleciona este também
```

+ Utilizado não só como seleção para substituição, mas também para navegar sincronizadamente pelos trechos de código onde estão as seleções.

### Uppercase no conteúdo selecionado
+ `Ctrl` + `Shift` + `U`;

### Lowercase no conteúdo selecionado
+ `Ctrl` + `U`;

### Comentario de linhas com  `//`
+ Comentar:
  + `Ctrl` + `K` + `C`

+ Descomentar:
  + `Ctrl` + `K` + `U`