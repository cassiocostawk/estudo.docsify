<font face="Calibri">

# ⭐ Filtro na Query da Api

[`⬆️ inicio`](../../Readme.md)

[`◀️ voltar`](../Readme.md)

---

+ Adicionar Argumento de filtro a partir da Query na Action da Controller:

    ```csharp
    [HttpGet]
    public IActionResult ListaDeLivros([FromQuery] LivroFiltro filtro) 
    ```

+ Aplicar o filtro informado no argumento na query da consulta:

    ```csharp
    var lista = _repo.All
        .AplicaFiltro(filtro) // <--
        .Select(l => l.ToApi())
        .ToList();
    ```

+ Discriminação do método de extensão `AplicaFiltro` que filtra o Query:

    ```csharp
    public static IQueryable<Livro> AplicaFiltro(this IQueryable<Livro> query, LivroFiltro filtro)
    {
        if (filtro != null)
        {
            if (!string.IsNullOrEmpty(filtro.Titulo))
            {
                query = query.Where(l => l.Titulo.Contains(filtro.Titulo));
            }

            if (!string.IsNullOrEmpty(filtro.Subtitulo))
            {
                query = query.Where(l => l.Subtitulo.Contains(filtro.Subtitulo));
            }

            if (!string.IsNullOrEmpty(filtro.Autor))
            {
                query = query.Where(l => l.Autor.Contains(filtro.Autor));
            }

            if (!string.IsNullOrEmpty(filtro.Lista))
            {
                query = query.Where(l => l.Lista == filtro.Lista.ParaTipo());
            }
        }

        return query;
    }
    ```

+ Modelo de classe DTO para disponibilizar campos para Filtro:

    ```csharp
    public class LivroFiltro
    {
        public string Autor { get; set; }
        public string Titulo { get; set; }
        public string Subtitulo { get; set; }
        public string Lista { get; set; }
    }
    ```

---

[`^ topo`](#Dev)
</font>
