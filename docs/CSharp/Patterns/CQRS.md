<font face="Calibri">

# 🧱 CQRS

[`⬆️ inicio`](../../Readme.md)

[`◀️ voltar`](../Readme.md)

---

## ❓ O que é CQRS

+ Sigla de Command-Query Responsibility Segregation

+ Padrão de desenvolvimento que separa as consultas `Queries` das ações que alteram o estado do sistema `Commands`

+ Implementado de diversas maneiras, mas geralmente fica na camada de aplicação. Também existem implementações dele com um projeto para `Commands`, e outro para `Queries`.

## 👍🏻 Prós/Contras

+ Devido a essa separação lógica, facilita o uso de mais de um banco de dados.
  Porém, essa decisão vem com diversos problemas, como a consistência de dados!

+ Melhora a legibilidade da aplicação, além de permitir maior separação de responsabilidades, e estimula separação de modelos.

## 🔗 Commands

+ Representam ações no sistema, que realizam alterações no estado dele.
+ Podem ser mapeadas de 1 para 1 a partir dos métodos de serviços de aplicação!
+ Geralmente são nomeados com o sufixo `Command`, ficando por exemplo `CreateProjectCommand`.
>
+ Os **Commands** contém as informações necessárias para ação, sendo similar ao modelo de entrada utilizados pelo serviço de aplicação
+ Para cada `Command` deverá existir um `CommandHandler`, que é a classe que lida com aquele comando

### Implementação

+ Os **Commands** contém as informações necessárias para ação, sendo similar ao modelo de entrada utilizados pelo serviço de aplicação.
+ Para cada `Command` deverá existir um `CommandHandler`, que é a classe que lida com aquele comando

## 🔗 Queries

+ Representam consultas no sistema, que **NÃO** realizam alterações no estado dele
+ Podem ser mapeadas de 1 para 1 a partir dos métodos de serviços de aplicação!
+ Geralmente são nomeados com o sufixo `Query`, ficando por exemplo `GetAllSkillsQuery`
>
+ As queries contém as informações necessárias para a consulta, por exemplo, contendo o identificador ou
parâmetros de busca.
+ Para cada `Query` deverá existir um `QueryHandler`, que é a classe que lida com aquela consulta

## Implementação

+ As queries contém as informações necessárias para a consulta, por exemplo, contendo o identificador ou parâmetros de busca.
+ Para cada `Query` deverá existir um `QueryHandler`, que é a classe que lida com aquela consulta

### Exemplo

#### `Command`

*Importante destacar que Handler tem a entrada é `CreateProjectCommand` e a saída é `int` com o novo ID.*

```csharp
// Command 
// + classe input model
public class CreateProjectCommand : IRequest<int>
{
    public string Title { get; set; }
    public string Description { get; set; }
    public int IdClient { get; set; }
    public int IdFreelancer { get; set; }
    public decimal TotalCost { get; set; }
}

// CommandHandler 
// + Classe que vai tratar as informações e efetivar esses dados no DB (como o Service estaria fazendo)
public class CreateProjectCommandHandler : IRequestHandler<CreateProjectCommand, int>
{
    // Poderia também estar usando Repository
    private readonly DevFreelaDbContext _dbContext; 

    public CreateProjectCommandHandler(DevFreelaDbContext dbContext)
    {
        _dbContext = dbContext;
    }

    public async Task<int> Handle(CreateProjectCommand request, CancellationToken cancellationToken)
    {
        var project = new Project(request.Title, request.Description, request.IdClient, request.IdFreelancer, request.TotalCost);

        await _dbContext.Projects.AddAsync(project);
        await _dbContext.SaveChangesAsync();

        return project.Id; // ao adiionar o Id estará correto nessa instancia
    }
}
```

#### `Query`

*Importante destacar que o Handler tem a entrada é `GetAllProjectsQuery` e a saída é `List<ProjectViewModel>>`*

```csharp
// Query
// + classe input model de uma query
public class GetAllProjectsQuery : IRequest<List<ProjectViewModel>>
{
    public string Query { get; private set; }

    public GetAllProjectsQuery(string query)
    {
        Query = query;
    }
}

// QueryHandler
// + classe que vai executar a consulta no DB
public class GetAllProjectsQueryHandler : IRequestHandler<GetAllProjectsQuery, List<ProjectViewModel>>
{
    private readonly DevFreelaDbContext _dbContext;

    public GetAllProjectsQueryHandler(DevFreelaDbContext dbContext)
    {
        _dbContext = dbContext;
    }

    public async Task<List<ProjectViewModel>> Handle(GetAllProjectsQuery request, CancellationToken cancellationToken)
    {
        var projects = _dbContext.Projects;
        var projectsViewModel = await projects.Select(p => new ProjectViewModel(p.Id, p.Title, p.CreatedAt)).ToListAsync();

        return projectsViewModel;
    }
}
```

#### Uso

*usando em uma `Controller` (ou onde será executado de fato):*

```csharp
/* Controller usando um Command e uma Query */
public class ProjectsController : ControllerBase
{
    private readonly IMediator _mediator;

    public ProjectsController(IMediator mediator)
    {
        _mediator = mediator;
    }

    /* usando uma Query */
    [HttpGet]
    public async Task<IActionResult> GetAll(string query)
    {
        var getAllProjectQuery = new GetAllProjectsQuery(query);

        var projects = await _mediator.Send(getAllProjectQuery); // <-- Query

        return Ok(projects);
    }

    /* usando um Command */
    [HttpPost]
    public async Task<IActionResult> Post([FromBody] CreateProjectCommand command)
    {
        if (command.Title.Length > 50)
            return BadRequest();

        var id = await _mediator.Send(command); // <-- Command

        return CreatedAtAction(nameof(GetById), id, command); 
    }

// ...
}
```

---

[`^ topo`](#Dev)
</font>
