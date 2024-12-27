# Interfaces de Repositório

[`🔼 início`](../Readme.md)

## IEntidadeRepository.cs

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

[`◀️ anterior`](./03%20AppDbContext.md) [`próximo ▶️`](./05%20Repositorio.md)