# Repositório

[`🔼 início`](../Readme.md)

## EntidadeRepository.cs

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

            int count = await query.Count();

            return new AsyncOutResult<IEnumerable<Entidade>, int>(query.ToList(), count);
        }
    }
}
```

---

[`◀️ anterior`](./04%20Interfaces%20de%20Repositorio.md) [`próximo ▶️`](./06%20Models.md)