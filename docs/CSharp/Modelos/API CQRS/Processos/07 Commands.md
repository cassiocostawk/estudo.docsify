# Commands

[`🔼 início`](../Readme.md)

## CreateEntidadeCommand.cs

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

## DeleteEntidadeCommand.cs

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

## UpdateEntidadeCommand.cs

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

[`◀️ anterior`](./06%20Models.md) [`próximo ▶️`](./08%20Queries.md)
