# Controllers

[`🔼 início`](../Readme.md)

## EntidadeController.cs

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
        public async Task<IActionResult> Create([FromForm] CreateEntidadeRequest request)
        {
            var command = new CreateEntidadeCommand(request);
            var response = await _mediator.Send(command);
            return Ok(response);
        }

        [HttpPut("{id}")]
        [HttpResultActionFilter]
        //[Authorize(Policy = "AtualizarEntidade")]
        public async Task<IActionResult> Update(Guid id, [FromForm] UpdateEntidadeRequest request)
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

[`◀️ anterior`](./08%20Queries.md) [`próximo ▶️`](./10%20Mapper.md)
