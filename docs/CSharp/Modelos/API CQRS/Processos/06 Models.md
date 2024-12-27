# Models

[`🔼 início`](../Readme.md)

`DTO, InputModels...`

> 📃 Exemplo:
> *CreateEntidadeRequest.cs, DeleteEntidadeRequest.cs, UpdateEntidadeRequest.cs, EntidadeResponse.cs*

## Filters

#### GetEntidadeRequestFilter.cs

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

## Requests

#### CreateEntidadeRequest.cs

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

#### DeleteEntidadeRequest.cs

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

#### UpdateEntidadeRequest.cs

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

## Responses

#### EntidadeResponse.cs

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

[`◀️ anterior`](./05%20Repositorio.md) [`próximo ▶️`](./07%20Commands.md)
