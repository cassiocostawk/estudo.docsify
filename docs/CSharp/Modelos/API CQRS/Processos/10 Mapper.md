# Mapper

[`🔼 início`](../Readme.md)

## alterar ResponseProfile.cs

```csharp
namespace Core.Mapping
{
    public class ResponseProfile : Profile
    {
        public ResponseProfile()
        {
            // ...

            CreateMap<Entidade, EntidadeResponse>()
                .ForMember(dest => dest.QuadraNome, opt => opt.MapFrom(x => x.Quadra.Nome))
                .ForMember(dest => dest.RuaNome, opt => opt.MapFrom(x => x.Rua.Nome))
                .ForMember(dest => dest.EquipamentoNome, opt => opt.MapFrom(x => x.Equipamento.Nome))
                .AfterMap((src, dest) =>
                {
                    dest.Id = src.Id != null ? src.Id.ToString().ToUpper() : String.Empty;
                    dest.DataCriacao = src.DataCriacao != null
                        ? src.DataCriacao.ToString("dd/MM/yyyy HH:mm:ss")
                        : String.Empty;
                    dest.DataEntidade = src.DataEntidade != null
                        ? src.DataEntidade.ToString("dd/MM/yyyy HH:mm:ss")
                        : String.Empty;
                    dest.DataRevisaoContentor = src.DataRevisaoContentor != null
                        ? src.DataRevisaoContentor.ToString("dd/MM/yyyy HH:mm:ss")
                        : String.Empty;
                    dest.DataRevisaoEslinga = src.DataRevisaoEslinga != null
                        ? src.DataRevisaoEslinga.ToString("dd/MM/yyyy HH:mm:ss")
                        : String.Empty;
                });

            // ...
        }
    }
}        
```

## alterar CommandProfile.cs

```csharp
namespace Core.Mapping
{

    public class CommandProfile : Profile
    {

        public CommandProfile()
        {
            // ...

            CreateMap<CreateEntidadeRequest, Entidade>()
                .ForMember(x => x.EntidadeArquivos, opt => opt.Ignore())
                .ForMember(x => x.DataEntidade, opt => opt.MapFrom(x => Convert.ToDateTime(x.DataEntidade, CultureInfo.GetCultureInfo("pt-BR"))))
                .ForMember(x => x.DataRevisaoContentor, opt => opt.MapFrom(x => Convert.ToDateTime(x.DataRevisaoContentor, CultureInfo.GetCultureInfo("pt-BR"))))
                .ForMember(x => x.DataRevisaoEslinga, opt => opt.MapFrom(x => Convert.ToDateTime(x.DataRevisaoEslinga, CultureInfo.GetCultureInfo("pt-BR"))))
                .ForMember(x => x.Itens, opt => opt.Ignore())
                .AfterMap((src, dest) =>
                {
                    dest.Aprovado = src.Aprovado.ToLower() == "true";
                    dest.Desmobilizado = src.Desmobilizado.ToLower() == "true";
                    dest.ManutencaoInterna = src.ManutencaoInterna.ToLower() == "true";
                    dest.ManutencaoExterna = src.ManutencaoExterna.ToLower() == "true";
                    dest.DataCriacao = DateTime.Now;
                    dest.EquipamentoId = new Guid(src.EquipamentoId);
                    dest.QuadraId = new Guid(src.QuadraId);
                    dest.RuaId = new Guid(src.RuaId);
                });

            // ...
        }
    }
}
```

---

[`◀️ anterior`](./09%20Controllers.md) [`próximo ▶️`](./11%20Permissoes.md)
