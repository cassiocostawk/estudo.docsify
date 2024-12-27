# Modelo de API

## Processo de desenvolvimento

### Entidades e Enums

#### Entidade.cs

```csharp
using Core.Enums;
using Core.Enums.Modulo;
using System;

namespace Core.Entities.Modulo
{
    public class Entidade : BaseEntity
    {
        // TODO: Campos
    }
}
```

#### EnumDestinoContentor.cs

```csharp
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Text;

namespace Core.Enums.Modulo
{
    public enum EnumDestinoContentor
    {
        Interno,
        Externo,
        Apto
    }
}
```

---

### ModelConfiguration

#### EntidadeModelConfiguration.cs

```csharp

using Core.Entities.Modulo;
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Metadata.Builders;
using System;
using System.Collections.Generic;
using System.Text;

namespace Infra.Data.ModelConfiguration.Modulo
{
    public class EntidadeModelConfiguration : IEntityTypeConfiguration<Entidade>
    {
        public void Configure(EntityTypeBuilder<Entidade> builder)
        {
            builder.ToTable(nameof(Entidade));

            builder.Property(x => x.Id).ValueGeneratedNever();

            // TODO: Campos | conferir - DB config
        }
    }
}

```

---

### Interfaces de Repositório

#### IEntidadeRepository.cs

```csharp

using Core.Entities.Modulo;
using Core.Entities.Rdo;
using Core.Helpers;
using System;
using System.Collections.Generic;
using System.Threading.Tasks;

namespace Core.Interfaces.Repositories.Modulo
{
    public interface IEntidadeRepository : IBaseRepository<Entidade>
    {
        Task<IList<Entidade>> Get(IEnumerable<Guid> ids);

        Task<IList<Entidade>> Search(string text, int? take, int? offSet);

        Task<AsyncOutResult<IEnumerable<Entidade>, int>> SearchAll(string text, int? take, int? offSet, string sortingProp = null, bool? ascending = null, string tableFilter = null, string searchColumn = null);

        Task<Entidade> GetEntidadeById(Guid id);

        Task<IEnumerable<Entidade>> GetEntidadeByFilter(string text);

    }
}
```

---

### Repositório

#### EntidadeRepository.cs

```csharp
using Core.Entities.Modulo;
using Core.Helpers;
using Core.Interfaces.Repositories.Modulo;
using Infra.Data.Helpers;
using Microsoft.EntityFrameworkCore;
using Newtonsoft.Json;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading.Tasks;

namespace Infra.Data.Repositories.Modulo

{
    public class EntidadeRepository : BaseRepository<Entidade>, IEntidadeRepository
    {
        private readonly AppDbContext _dbContext;
        private readonly Guid _unidadeAcessoSelecionada;

        public EntidadeRepository(AppDbContext dbContext) : base(dbContext) 
        {
            _dbContext = dbContext;
            _unidadeAcessoSelecionada = GetSelectedAccessUnitId();
        }

        public async Task<IList<Entidade>> Get(IEnumerable<Guid> ids)
        {
            var query = _dbContext.Entidade.AsQueryable();

            if (ids != null) query = query.Where(x => x.UnidadeAcessoId == _unidadeAcessoSelecionada && ids.Contains(x.Id));

            return await query.ToListAsync();
        }

        public async Task<IEnumerable<Entidade>> GetEntidadeByFilter(string text)
        {
            var query = _dbContext.Set<Entidade>().AsQueryable();

            query = query.Where(x => x.UnidadeAcessoId == _unidadeAcessoSelecionada);

            return await query.ToListAsync();                
        }

        public async Task<Entidade> GetEntidadeById(Guid id)
        {
            var query = _dbContext.Set<Entidade>().AsQueryable()
                .Where(x => x.UnidadeAcessoId == _unidadeAcessoSelecionada &&
                            x.Id.Equals(id));

            return await query.FirstOrDefaultAsync();
        }

        public async Task<IList<Entidade>> Search(string text, int? take, int? offSet)
        {
            var query = _dbContext.Entidade.AsQueryable().
                Where(x => x.UnidadeAcessoId == _unidadeAcessoSelecionada && 
                          (x.Origem.Contains(text)));

            // TODO: Campos | Filtro | conferir acima

            if (take != null && offSet != null) 
                query = query.Skip((int)offSet).Take((int)take);
            
            return await query.ToListAsync();

        }

        public async Task<AsyncOutResult<IEnumerable<Entidade>, int>> SearchAll(string text,
            int? take,
            int? offSet,
            string sortingProp = null,
            bool? ascending = null,
            string tableFilter = null,
            string searchColumn = null
        )
        {
            text = text ?? string.Empty;

            var query = _dbContext.Entidade.AsQueryable().
                Where(x => x.UnidadeAcessoId == _unidadeAcessoSelecionada);

            #region Pesquisar
            if (!String.IsNullOrEmpty(text) && (String.IsNullOrEmpty(searchColumn) || searchColumn.ToUpper() == "TODOS"))
            {
                query = query.Where(x => x.Origem.Contains(text)
                    // TODO: Campos | Filtro | todos campos
                
                );
            }

            if (!String.IsNullOrEmpty(text) && !String.IsNullOrEmpty(searchColumn) && searchColumn.ToUpper() != "TODOS")
            {
                switch (searchColumn.ToUpper())
                {
                    case "ORIGEM":
                        query = query.Where(x => x.Origem.Contains(text));
                        break;

                    // TODO: Campos | Filtro | campo a campo
                    default:
                        break;
                }
            }
            #endregion

            #region Filtro
            IList<KeyValuePair<string, string>> tableFilterList = new List<KeyValuePair<string, string>>();

            if (!string.IsNullOrEmpty(tableFilter))
                tableFilterList = JsonConvert.DeserializeObject<List<KeyValuePair<string, string>>>(tableFilter);

            if (tableFilterList.Count > 0)
            {
                tableFilterList = tableFilterList
                    .Where(x => !string.IsNullOrEmpty(x.Value))
                    .ToList();

                query = query.ApplyFilters(tableFilterList);
            }
            #endregion

            #region Ordenacao
            if (!string.IsNullOrEmpty(sortingProp) && ascending != null)
            {
                if (DataHelpers.CheckExistingProperty<Entidade>(sortingProp))
                    query = query.OrderByDynamic(sortingProp, (bool)ascending);
            }
            else
                query = query.OrderByDynamic("DataCriacao", false);
            #endregion

            #region Paginacao
            if (take != null && offSet != null)
                query = query.Skip((int)offSet).Take((int)take);
            #endregion

            int count = await query.CountAsync();

            return new AsyncOutResult<IEnumerable<Entidade>, int>(await query.ToListAsync(), count);
        }
    }
}
```

---

### Commands

#### CreateEntidadeCommand.cs

```csharp
using AutoMapper;
using Core.Helpers;
using Core.Interfaces.Repositories.Modulo;
using Core.Models.Requests.Modulo.Entidade;
using Core.Models.Responses.Modulo;
using MediatR;
using System.Threading;
using System.Threading.Tasks;

namespace Core.Commands.Modulo.Entidade
{
    public class CreateEntidadeCommand : IRequest<Result<EntidadeResponse>>
    {
        public CreateEntidadeRequest Request;

        public CreateEntidadeCommand(CreateEntidadeRequest request)
        {
            Request = request;
        }
    }

    public class CreateEntidadeCommandHandler : IRequestHandler<CreateEntidadeCommand, Result<EntidadeResponse>>
    {
        private readonly IEntidadeRepository _repository;
        private readonly IMapper _mapper;

        public CreateEntidadeCommandHandler(IEntidadeRepository repository, IMapper mapper)
        {
            _repository = repository;
            _mapper = mapper;
        }

        public async Task<Result<EntidadeResponse>> Handle(CreateEntidadeCommand request, CancellationToken cancellationToken)
        {
            var result = new Result<EntidadeResponse>();

            // TODO: Validacao | Create

            var model = _mapper.Map<Entities.Modulo.Entidade>(request.Request);
            model.UnidadeAcessoId = await _repository.GetSelectedAccessUnitIdAsync();
            var registro = await _repository.AddAsync(model);

            EntidadeResponse response = _mapper.Map<EntidadeResponse>(registro);

            result.Value = response;

            return result;
        }
    }
}
```

#### DeleteEntidadeCommand.cs

```csharp
using Core.Helpers;
using Core.Interfaces.Repositories.Modulo;
using Core.Models.Requests.Modulo.Entidade;
using Core.Models.Responses.Modulo;
using MediatR;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading;
using System.Threading.Tasks;

namespace Core.Commands.Modulo.Entidade
{
    public class DeleteEntidadeCommand : IRequest<Result>
    {
        public DeleteEntidadeRequest Request;

        public DeleteEntidadeCommand(DeleteEntidadeRequest request)
        {
            Request = request;
        }
    }

    public class DeleteEntidadeCommandHandler : IRequestHandler<DeleteEntidadeCommand, Result>
    {
        private readonly IEntidadeRepository _repository;

        public DeleteEntidadeCommandHandler(IEntidadeRepository repository)
        {
            _repository = repository;
        }

        public async Task<Result> Handle(DeleteEntidadeCommand request, CancellationToken cancellationToken)
        {
            var result = new Result();

            if (request.Request.Ids == null)
            {
                result.WithError("Parâmetro inválido.");
                return result;
            }

            var registros = await _repository.Get(request.Request.Ids.ToArray());

            if (registros.Count == 0)
            {
                result.WithNotFound("Registros não encontrados.");
                return result;
            }

            // TODO: Validacao | Verificar relacionados antes de excluir

            await _repository.SoftDeleteRangeAsync(registros);
            return result;
        }
    }
}
```

#### UpdateEntidadeCommand.cs

```csharp
using AutoMapper;
using Core.Helpers;
using Core.Interfaces.Repositories.Modulo;
using Core.Models.Requests.Modulo.Entidade;
using Core.Models.Responses.CheckLists;
using Core.Models.Responses.Modulo;
using MediatR;
using System;
using System.Collections.Generic;
using System.Text;
using System.Threading;
using System.Threading.Tasks;

namespace Core.Commands.Modulo.Entidade
{
    public class UpdateEntidadeCommand : IRequest<Result<EntidadeResponse>>
    {
        public Guid Id { get; set; }
        public UpdateEntidadeRequest Request;

        public UpdateEntidadeCommand(Guid id, UpdateEntidadeRequest request)
        {
            Id = id;
            Request = request;
        }
    }

    public class UpdateEntidadeCommandHandler : IRequestHandler<UpdateEntidadeCommand, Result<EntidadeResponse>>
    {
        private readonly IEntidadeRepository _repository;
        private readonly IMapper _mapper;

        public UpdateEntidadeCommandHandler(IEntidadeRepository repository, IMapper mapper)
        {
            _repository = repository;
            _mapper = mapper;
        }

        public async Task<Result<EntidadeResponse>> Handle(UpdateEntidadeCommand request, CancellationToken cancellationToken)
        {
            var result = new Result<EntidadeResponse>();

            // TODO: Validacoes | Update

            var oldData = await _repository.GetById(request.Id);

            if (oldData == null)
            {
                result.WithNotFound("Registro não encontrado!");
                return result;
            }

            var newData = _mapper.Map(request.Request, oldData);

            if (await _repository.UpdateAsync(newData))
                result.Value = _mapper.Map<EntidadeResponse>(newData);

            return result;
        }
    }
}
```

---

### Queries

#### GetEntidadeQuery.cs

```csharp
using AutoMapper;
using Core.Helpers;
using Core.Interfaces.Repositories.Modulo;
using Core.Models.Filters.Modulo;
using Core.Models.Responses.Modulo;
using MediatR;
using System.Collections.Generic;
using System.Linq;
using System.Threading;
using System.Threading.Tasks;

namespace Core.Queries.Modulo.Entidade
{
    public class GetEntidadeQuery : IRequest<Result<IEnumerable<EntidadeResponse>>>
    {
        public GetEntidadeRequestFilter Filter;

        public GetEntidadeQuery(GetEntidadeRequestFilter filter)
        {
            Filter = filter;
        }
    }

    public class GetEntidadeQueryHandler : IRequestHandler<GetEntidadeQuery, Result<IEnumerable<EntidadeResponse>>>
    {
        private readonly IEntidadeRepository _repository;
        private readonly IMapper _mapper;

        public GetEntidadeQueryHandler(IEntidadeRepository repository, IMapper mapper)
        {
            _repository = repository;
            _mapper = mapper;
        }

        public async Task<Result<IEnumerable<EntidadeResponse>>> Handle(GetEntidadeQuery query, CancellationToken cancellationToken)
        {
            var result = new Result<IEnumerable<EntidadeResponse>>();
            var unidadeAcessoId = await _repository.GetSelectedAccessUnitIdAsync();
            
            var registros = await _repository.SearchAll(
                query.Filter.Text, 
                query.Filter.Take, 
                query.Filter.Skip, 
                query.Filter.SortingProp,
                query.Filter.Ascending,
                query.Filter.TableFilter, 
                query.Filter.SearchColumn);

            var response = _mapper.Map<IEnumerable<EntidadeResponse>>(registros);

            result.Value = response;
            result.Count = response.Count();
            result.TableCount = await _repository.CountTable();

            return result;
        }
    }
}
```

#### GetEntidadeByIdQuery.cs

```csharp
using AutoMapper;
using Core.Helpers;
using Core.Interfaces.Repositories.Modulo;
using Core.Models.Responses.Modulo;
using MediatR;
using System;
using System.Collections.Generic;
using System.Text;
using System.Threading;
using System.Threading.Tasks;

namespace Core.Queries.Modulo.Entidade
{
    public class GetEntidadeByIdQuery : IRequest<Result<EntidadeResponse>>
    {
        public Guid Id { get; set; }

        public GetEntidadeByIdQuery(Guid id)
        {
            Id = id;
        }
    }

    public class GetEntidadeByIdQueryHandler : IRequestHandler<GetEntidadeByIdQuery, Result<EntidadeResponse>>
    {
        private readonly IEntidadeRepository _repository;
        private readonly IMapper _mapper;

        public GetEntidadeByIdQueryHandler(IEntidadeRepository repository, IMapper mapper)
        {
            _repository = repository;
            _mapper = mapper;
        }

        public async Task<Result<EntidadeResponse>> Handle(GetEntidadeByIdQuery request, CancellationToken cancellationToken)
        {
            var result = new Result<EntidadeResponse>();
            var registro = await _repository.GetEntidadeById(request.Id);

            if (registro == null)
            {
                result.WithNotFound("Registro não encontrado!");
                return result;
            }

            result.Value = _mapper.Map<EntidadeResponse>(registro);
            return result;
        }
    }
}
```

#### GetEntidadeByFilterQuery.cs

```csharp
using AutoMapper;
using Core.Helpers;
using Core.Interfaces.Repositories.Modulo;
using Core.Models.Filters;
using Core.Models.Responses.Modulo;
using MediatR;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading;
using System.Threading.Tasks;

namespace Core.Queries.Modulo.Entidade
{
    public class GetEntidadeByFilterQuery : IRequest<Result<IEnumerable<EntidadeResponse>>>
    {
        public BaseRequestFilter Filter { get; set; }

        public GetEntidadeByFilterQuery(BaseRequestFilter filter)
        {
            Filter = filter;
        }
    }

    public class GetEntidadeByFilterQueryHandler : IRequestHandler<GetEntidadeByFilterQuery, Result<IEnumerable<EntidadeResponse>>>
    {
        private readonly IEntidadeRepository _repository;
        private readonly IMapper _mapper;

        public GetEntidadeByFilterQueryHandler(IEntidadeRepository repository, IMapper mapper)
        {
            _repository = repository;
            _mapper = mapper;
        }

        public async Task<Result<IEnumerable<EntidadeResponse>>> Handle(GetEntidadeByFilterQuery request, CancellationToken cancellationToken)
        {
            var result = new Result<IEnumerable<EntidadeResponse>>();
            var entidade = await _repository.GetEntidadeByFilter(request.Filter.Text);

            if (entidade == null || !entidade.Any())
            {
                result.WithNotFound("Entidade não encontrada!");
                return result;
            }

            result.Value = _mapper.Map<IEnumerable<EntidadeResponse>>(entidade);

            return result;
        }
    }
}
```

---

### Models

`DTO, InputModels...`

*Exemplo:   CreateEntidadeRequest.cs, DeleteEntidadeRequest.cs, UpdateEntidadeRequest.cs, EntidadeResponse.cs*

#### Filters

##### GetEntidadeRequestFilter.cs

```csharp
using Core.Enums.Modulo;
using Core.Enums;
using System;

namespace Core.Models.Filters.Modulo
{
    public class GetEntidadeRequestFilter : BaseRequestFilter
    {
        public string Origem { get; set; }
        public DateTime DataRevisao { get; set; }
        public EnumInternoExterno TipoManutencao { get; set; }
        public EnumDestinoContentor DestinoContentor { get; set; }

        // TODO: Campos | conferencia
    }
}
```

#### Requests

##### CreateEntidadeRequest.cs

```csharp
using Core.Entities.Modulo;
using Core.Enums.Modulo;
using Core.Enums;
using System;

namespace Core.Models.Requests.Modulo.Entidade
{
    public class CreateEntidadeRequest
    {
        public string Origem { get; set; }
        public ModuloEquipamento Equipamento { get; set; }
        public int EquipamentoId { get; set; }
        public DateTime DataRevisao { get; set; }
        public int EslingaId { get; set; }
        public int ReparoId { get; set; }
        public int QuadraId { get; set; }
        public int RuaId { get; set; }
        public bool Aprovado { get; set; }
        public bool Desmobilizado { get; set; }
        public EnumInternoExterno TipoManutencao { get; set; }
        public int ResponsavelId { get; set; }
        public int EquipeId { get; set; }
        public string Observacao { get; set; }
        public EnumDestinoContentor DestinoContentor { get; set; }

        // TODO: Campos | conferencia
    }
}
```

##### DeleteEntidadeRequest.cs

```csharp
using System;
using System.Collections.Generic;
using System.Text;

namespace Core.Models.Requests.Modulo.Entidade
{
    public class DeleteEntidadeRequest
    {
        public ICollection<Guid> Ids { get; set; }
    }
}
```

##### UpdateEntidadeRequest.cs

```csharp
using Core.Entities.Modulo;
using Core.Enums.Modulo;
using Core.Enums;
using System;

namespace Core.Models.Requests.Modulo.Entidade
{
    public class UpdateEntidadeRequest
    {
        public string Origem { get; set; }
        public ModuloEquipamento Equipamento { get; set; }
        public int EquipamentoId { get; set; }
        public DateTime DataRevisao { get; set; }
        public int EslingaId { get; set; }
        public int ReparoId { get; set; }
        public int QuadraId { get; set; }
        public int RuaId { get; set; }
        public bool Aprovado { get; set; }
        public bool Desmobilizado { get; set; }
        public EnumInternoExterno TipoManutencao { get; set; }
        public int ResponsavelId { get; set; }
        public int EquipeId { get; set; }
        public string Observacao { get; set; }
        public EnumDestinoContentor DestinoContentor { get; set; }
    }
}
```

#### Responses

##### EntidadeResponse.cs

```csharp
using Core.Entities.Modulo;
using Core.Enums.Modulo;
using Core.Enums;
using System;

namespace Core.Models.Responses.Modulo
{
    public class EntidadeResponse
    {
        public string Origem { get; set; }
        public ModuloEquipamento Equipamento { get; set; }
        public int EquipamentoId { get; set; }
        public DateTime DataRevisao { get; set; }
        public int EslingaId { get; set; }
        //DataRevisãoEslinga
        public int ReparoId { get; set; }
        public int QuadraId { get; set; }
        public int RuaId { get; set; }
        public bool Aprovado { get; set; }
        public bool Desmobilizado { get; set; }
        public EnumInternoExterno TipoManutencao { get; set; }
        public int ResponsavelId { get; set; }
        public int EquipeId { get; set; }
        //Fotos
        public string Observacao { get; set; }
        public EnumDestinoContentor DestinoContentor { get; set; }
    }
}
```

---

### Controllers

#### EntidadeController.cs

```csharp

using Core.Commands.Modulo.Entidade;
using Core.Models.Filters.Modulo;
using Core.Models.Requests.Modulo.Entidade;
using Core.Queries.Modulo.Entidade;
using MediatR;
using Microsoft.AspNetCore.Authorization;
using Microsoft.AspNetCore.Mvc;
using System;
using System.Threading.Tasks;
using Web.Api;
using Web.Filters;

namespace Web.Controllers.Modulo
{
    [Authorize]
    public class EntidadeController : BaseApiController
    {
        private readonly IMediator _mediator;

        public EntidadeController(IMediator mediator)
        {
            _mediator = mediator;
        }
        
        [HttpGet]
        [HttpResultActionFilter]
        // TODO: Authorization | [Authorize(Policy = "ConsultarEntidade")]
        public async Task<IActionResult> Get([FromQuery] GetEntidadeRequestFilter filter)
        {
            var command = new GetEntidadeQuery(filter);
            var response = await _mediator.Send(command);
            return Ok(response);
        }
        
        [HttpGet("Search")]
        [HttpResultActionFilter]
        //[Authorize(Policy = "ConsultarEntidade")]
        public async Task<IActionResult> Search([FromQuery] GetEntidadeRequestFilter filter)
        {
            var command = new GetEntidadeByFilterQuery(filter);
            var response = await _mediator.Send(command);
            return Ok(response);
        }

        [HttpGet("{id}")]
        [HttpResultActionFilter]
        //[Authorize(Policy = "ConsultarEntidade")]
        public async Task<IActionResult> GetById(Guid id)
        {
            var command = new GetEntidadeByIdQuery(id);
            var response = await _mediator.Send(command);
            return Ok(response);
        }

        [HttpPost]
        [HttpResultActionFilter]
        //[Authorize(Policy = "CadastrarEntidade")]
        public async Task<IActionResult> Create([FromBody] CreateEntidadeRequest request)
        {
            var command = new CreateEntidadeCommand(request);
            var response = await _mediator.Send(command);
            return Ok(response);
        }

        [HttpPut("{id}")]
        [HttpResultActionFilter]
        //[Authorize(Policy = "AtualizarEntidade")]
        public async Task<IActionResult> Update(Guid id, [FromBody] UpdateEntidadeRequest request)
        {

            var command = new UpdateEntidadeCommand(id, request);
            var response = await _mediator.Send(command);
            return Ok(response);
        }

        [HttpPost("Delete")]
        [HttpResultActionFilter]
        //[Authorize(Policy = "DeletarEntidade")]
        public async Task<IActionResult> Delete([FromBody] DeleteEntidadeRequest request)
        {
            var command = new DeleteEntidadeCommand(request);
            var response = await _mediator.Send(command);
            return Ok(response);
        }
    }
}
```

---

### Adicionar no DbContext

#### alterar AppDbContext.cs

```csharp
public virtual DbSet<Entidade> Entidade { get; set; }

// ...

protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    // ...

    modelBuilder.ApplyConfiguration(new EntidadeModelConfiguration());

    // ...
}
```
