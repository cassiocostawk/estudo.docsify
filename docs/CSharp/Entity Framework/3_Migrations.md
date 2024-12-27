<font face="Calibri">

# 🛢️ Migrations

[`⬆️ inicio`](../../Readme.md)

[`◀️ voltar`](../Readme.md)

---

+ Para usar os comandos no **Console do Gerenciador de Pacotes**, instalar o pacote `Microsoft.EntityFrameworkCore.Tools` no projeto.

| Cmdlet__________ | Description | Informação | Passo |
| :--- | :--- | :--- | --- |
| Add-Migration | Adds a new migration. | Gerar versão da  Migrarion no estado atual das entidades. | 1º |
| Drop-Database | Drops the database. | Esse comando é utilizado para dropar o banco de dados apontado pelo contexto. | 1º |
| Remove-Migration | Removes the last migration. | Esse comando é utilizado para remover a última migração não aplicada no banco de dados apontado pelo contexto. | 2º |
| Scaffold-DbContext | Scaffolds a DbContext and entity types for a database. | Esse comando é utilizado para criar uma classe que estende de DbContext, além de classes que representam as tabelas do banco. | |
| Script-Migration | Generates a SQL script from migrations. | Gerar o DDL que vai atualizar o DB, será gerado o script para ser executado externamente. | 2º |
| Update-Database | Updates the database to a specified migration. | Faz a atuaização diretamente no DB. | 2º |

+ `Update-Database`
  + `-Verbose` exibe um log de atualização.
  + `MeuMigration` - Supondo que anteriormente executei o comando `Add-Migration MeuMigration`, sera feito um update somente do Migration com o nome `MeuMigration` mesmo que tenha outros a executar.

+ `__EFMigrationsHistory` é a tabela que tem registrada as versões do Migration já executadas,
  como um versionador de scripts de upgrade de DB.
  
| Migration | Product Version |
| :--- | :--- |
| 20220131224752_MeuMigration | 1.1.2 |
| 20220131224910_AddUnidade | 1.1.2 |

+ Migrations só são geradas pra entidades que tem `DbSet<T>` informado no `DbContext`.

```csharp
public class LojaContext : DbContext
{
    public DbSet<Produto> Produtos { get; set; } // <--
    public DbSet<Compra> Compras { get; set; }   // <--
    
    //...
}
```

---

## Migrate pela própria aplicação

+ Isso é feito através do método de extensão `Migrate`, que está acessível na propriedade `Database` da classe `DbContext`.

Exemplo:

```csharp
//Instalar Microsoft.EntityFrameworkCore.Relational

using(var contexto = new LojaContext())
{
  contexto.Database.Migrate();
}
```

+ *Você precisa garantir que esse código seja executado antes de qualquer acesso aos objetos gerenciados pelo contexto. Isso vai depender do tipo de aplicação que será implementada.*

### Mapeamento de Properties automático

+ A convenção do Entity para descobrir as colunas de um tipo é procurar por todas propriedades públicas que possuem getters e setters.

---

[`^ topo`](#⭐-migrations)
</font>
