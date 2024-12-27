<font face="Calibri">

# ⭐ Paginação na Query da Api

[`⬆️ inicio`](../../Readme.md)

[`◀️ voltar`](../Readme.md)

---

+ Adicionar Argumento de ordenação a partir da Query na Action da Controller:

    ```csharp
    [HttpGet]
    public IActionResult ListaDeLivros(
        [FromQuery] LivroFiltro filtro, 
        [FromQuery] LivroOrdem ordem,
        [FromQuery] LivroPaginacao paginacao) // <---
    ```

+ Ao invéz de retornar uma lista, deve-se retornar uma lista paginada:

    ```json
    {
        "total": 6,
        "totalPaginas": 2,
        "tamanhoPagina": 4,
        "numeroPagina": 1,
        "resultado": [
            {
                "id": 1,
                "titulo": "Livro 1",
                "subtitulo": "Sub1",
                "autor": "Lorem Ipsum",
                "resumo": "teste 1",
                "capa": "/api/livros/1/capa",
                "lista": "Lendo"
            },
            {
                "id": 2,
                "titulo": "Livro 2",
                "subtitulo": "Sub2",
                "autor": "AlaTest",
                "resumo": "teste de resumo 2",
                "capa": "/api/livros/2/capa",
                "lista": "Lidos"
            },
            {
                "id": 1002,
                "titulo": "Livro 2",
                "subtitulo": "Sub2",
                "autor": "Neoway",
                "resumo": "teste de resumo",
                "capa": "/api/livros/1002/capa",
                "lista": "ParaLer"
            },
            {
                "id": 4002,
                "titulo": "Livro 3 insert",
                "subtitulo": "Sub3",
                "autor": "GNRE",
                "resumo": "dfgbdgb",
                "capa": "/api/livros/4002/capa",
                "lista": "ParaLer"
            }
        ],
        "anterior": "",
        "proximo": "livros?tamanho=4&pagina=2"
    }
    ```

+ Então deve-se criar uma classe de paginação:

    ```csharp
    public class LivroPaginacao
    {
        public int Pagina { get; set; } = 1;
        public int Tamanho { get; set; } = 25;
    }
    ```

+ Criar a classe que irá retornar com informações da paginação e conteúdo:

    ```csharp
    public class LivroPaginado
    {
        public int Total { get; set; }
        public int TotalPaginas { get; set; }
        public int TamanhoPagina { get; set; }
        public int NumeroPagina { get; set; }
        public IList<LivroApi> Resultado { get; set; }
        public string Anterior { get; set; }
        public string Proximo { get; set; }
    }
    ```

+ Método de extensão que irá paginar o conteúdo:

    ```csharp
    public static LivroPaginado ToLivroPaginado(this IQueryable<LivroApi> query, LivroPaginacao paginacao)
    {
        int totalItens = query.Count();
        int totalPaginas = (int)Math.Ceiling(totalItens / (double)paginacao.Tamanho);

        return new LivroPaginado {
            Total = totalItens,
            TotalPaginas = totalPaginas,
            NumeroPagina = paginacao.Pagina,
            TamanhoPagina = paginacao.Tamanho,
            Resultado = query
                .Skip(paginacao.Tamanho * (paginacao.Pagina - 1))
                .Take(paginacao.Tamanho)
                .ToList(),
            Anterior = (paginacao.Pagina > 1) ? $"livros?tamanho={paginacao.Tamanho}&pagina={paginacao.Pagina - 1}" : "",
            Proximo = (paginacao.Pagina < totalPaginas) ? $"livros?tamanho={paginacao.Tamanho}&pagina={paginacao.Pagina + 1}" : ""

        };
    }    
    ```

+ Alterar o código da Action para fazer a paginação e retorná-la:

    ```csharp
    var livroPaginado = _repo.All
        .AplicaFiltro(filtro)
        .AplicaOrdem(ordem)
        .Select(l => l.ToApi())
        .ToLivroPaginado(paginacao); // <-- gerar tipo paginado

    return Ok(livroPaginado); // <-- retornar tipo paginado
    ```

---

[`^ topo`](#⭐-paginação-na-query-da-api)
</font>
