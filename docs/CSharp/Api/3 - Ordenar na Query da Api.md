<font face="Calibri">

# ⭐ Ordenar na Query da Api

[`⬆️ inicio`](../../Readme.md)

[`◀️ voltar`](../Readme.md)

---

+ Adicionar Argumento de ordenação a partir da Query na Action da Controller:

    ```csharp
    [HttpGet]
    public IActionResult ListaDeLivros(
        [FromQuery] LivroFiltro filtro, 
        [FromQuery] LivroOrdem ordem) // <--
    ```

+ Aplicar a ordenação informado no argumento na query da consulta:

    ```csharp
    var lista = _repo.All
        .AplicaFiltro(filtro)
        .AplicaOrdem(ordem) // <---
        .Select(l => l.ToApi())
        .ToList();
    ```

+ *Discriminação do método AplicaOrdem que filtra o Query:*

    ```csharp
    public static IQueryable<Livro> AplicaOrdem(this IQueryable<Livro> query, LivroOrdem ordem)
    {
        if (ordem != null)
        {
            if (!string.IsNullOrEmpty(ordem.OrdenarPor))
                query = query.OrderBy(ordem.OrdenarPor);
        }

        return query;
    }
    ```

+ *Para usar a Ordenação por `string` e permitir ordenação **Decrescente** é necessário instalar o pacote `System.Linq.Dynamic.Core`*.
<br>

+ *Modelo de classe para disponibilizar campos para Filtro:*

    ```csharp
    public class LivroOrdem
    {
        public string OrdenarPor { get; set; }
    }
    ```

---

[`^ topo`](#⭐-ordenar-na-query-da-api)
</font>
