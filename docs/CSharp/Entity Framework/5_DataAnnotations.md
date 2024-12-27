<font face="Calibri">

# ⭐ Data Annotations

[`⬆️ inicio`](../../README.md)

[`◀️ voltar`](../Readme.md)

---

+ Vantagens:
  + Apenas adicionar acima dos objetos da entidade.
  + Utilizado para validação via ASP.Net MVC
+ Desvantagens:
  + É menos completo que fazer o mapeamento no `DbContext` (Fluent API).
  + Algumas anotations são atreladas a um DB, oque pode causar refatoração em caso de troca de DB da aplicação.

## Mapeando DB legado

+ Explicitar a Tabela e Colunas na classe:

```csharp
[Table("Actor")] // <--
public class Ator
{
    [Column("actor_id")]          // <--
    public int Id { get; set; }
    [Column("first_name")]        // <--
    public string PrimeiroNome { get; set; }
    [Column("last_name")]         // <--
    public string UltimoNome { get; set; }
}
```

+ Explicitar tipo de dados/size:

```csharp
[Column("first_name", TypeName = "varchar(45)")]
public string PrimeiroNome { get; set; }
[Column("last_name", TypeName = "varchar(45)")]
public string UltimoNome { get; set; }
```

*Não é o ideal pois `varchar(45)` pode servir somente para o DB atual, sendo assim, se necessário mudar o provedor de DB futuramente isso poderia mudar.*

+ Nullidade de uma coluna:
  + A nuliade de uma coluna é determinada pelo tipo da CLR da propriedade.
  + Caso seja permitido o valor null pelo tipo de CLR, a coluna terá o valor null.
  + Exemplos de tipos (classe) que permitem:
    + `string = null;`
  + Exemplos de Tipos (primitivos) que NÃO permitem:
    + `int != null;`
    + `double != null;`
    + `boolean != null;`
    + `DateTime != null;` (struct)
  + Observa-se que os tipos primitivos não aceitam null, e como string na verdade é uma classe ela aceita.
  + Para resolver isso podemos anotar na propriedade da classe como `Required`:

```csharp
[Required] // <--
[Column("first_name", TypeName = "varchar(45)")]
public string PrimeiroNome { get; set; }
```

> **ASP.Net MVC e Data Annotations**
> O ASP.NET MVC é um framework que usa esse recurso para fazer a validação dos objetos criados no processo de Model Binding. Porém, nem todas anotações são utilizadas para esse propósito. Abaixo listo duas anotações que são usadas para validar objetos no processo de Model Binding pelo ASP.NET MVC:
>
> + `[Required]`
> + `[StringLength]`

---

### Não mapear Property

+ Usando DataAnnotations `[NotMapped]`.

  ```csharp
  public class Compra
  {
      //...outras propriedades...
      [NotMapped]                             
      public double PrecoTotal { get; set; }  // <-- 
  }
  ```

+ Removendo o Get e Set da Propery
  + a convenção para descoberta de colunas procura por propriedades públicas com ambos getter e setter
+ Usar o método `Ignore` no `OnModelCreating`
  + como mostrado em `.\6 - Model Creating - Fluent API.md`.

---

[`^ topo`](#⭐-data-annotations)
</font>
