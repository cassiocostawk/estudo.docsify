# Queries

[`🔼 início`](../Readme.md)

## GetEntidadeQuery.cs

```csharp
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

        var response = registros.Result(out _).Select(x => _mapper.Map<EntidadeResponse>(x));

        result.Value = response;
        result.Count = response.Count();
        result.TableCount = await _repository.CountTable();

        return result;
    }
}
```

## GetEntidadeByIdQuery.cs

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

## GetEntidadeByFilterQuery.cs

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

[`◀️ anterior`](./07%20Commands.md) [`próximo ▶️`](./09%20Controllers.md)
