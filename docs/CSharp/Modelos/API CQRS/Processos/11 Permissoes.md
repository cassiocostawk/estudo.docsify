# Permissões

[`🔼 início`](../Readme.md)

## alterar PolicyExtension.cs

```csharp
namespace Web.Extension
{
    public static class PolicyExtension
    {
        public static IServiceCollection AddPolicy(this IServiceCollection services)
        {
            // ... criar Enums com Ctrl + .

            #region Entidade
            options.AddPolicy(EnumPermissao.ConsultarEntidade.ToString(),
                policy => policy.Requirements.Add(
                    new PolicyRequirement(EnumPermissao.ConsultarEntidade.ToString())));
            options.AddPolicy(EnumPermissao.CadastrarEntidade.ToString(),
                policy => policy.Requirements.Add(
                    new PolicyRequirement(EnumPermissao.CadastrarEntidade.ToString())));
            options.AddPolicy(EnumPermissao.AtualizarEntidade.ToString(),
                policy => policy.Requirements.Add(
                    new PolicyRequirement(EnumPermissao.AtualizarEntidade.ToString())));
            options.AddPolicy(EnumPermissao.DeletarEntidade.ToString(),
                policy => policy.Requirements.Add(
                    new PolicyRequirement(EnumPermissao.DeletarEntidade.ToString())));

            #endregion                

            // ...
        }
    }
}
```

## alterar EnumPermissao

```csharp
namespace Core.Enums.Security
{
    public enum EnumPermissao
    {
        // ... alterar criado por Ctrl + .

        // Entidade
        [Description("ConsultarEntidade")] ConsultarEntidade,
        [Description("CadastrarEntidade")] CadastrarEntidade,
        [Description("AtualizarEntidade")] AtualizarEntidade,
        [Description("DeletarEntidade")] DeletarEntidade,

        // ...
    }
}
```

## alterar PermissaoModelConfiguration

> ###❗ Atenção
> **após criar essas permissões é necessário executar o Migration**
> para serem gerados os scripts que incluem as permissões

```csharp
namespace Infra.Data.ModelConfiguration.Security
{
    public class PermissaoModelConfiguration : IEntityTypeConfiguration<Permissao>
    {
        public void Configure(EntityTypeBuilder<Permissao> entityTypeBuilder)
        {
            entityTypeBuilder.HasData(
            // ...

            #region [ENTIDADE]
                new Permissao()
                {
                    Id = new Guid(),
                    Nome = "ConsultarEntidade",
                    Descricao = "Consultar Entidade",
                    TipoUsuario = 0,
                    DataCriacao = new DateTime(2023, 10, 5, 0, 0, 0, DateTimeKind.Local),
                    Deletado = false
                },
                new Permissao()
                {
                    Id = new Guid(),
                    Nome = "CadastrarEntidade",
                    Descricao = "Cadastrar Entidade",
                    TipoUsuario = 0,
                    DataCriacao = new DateTime(2023, 10, 5, 0, 0, 0, DateTimeKind.Local),
                    Deletado = false
                },
                new Permissao()
                {
                    Id = new Guid(),
                    Nome = "AtualizarEntidade",
                    Descricao = "Atualizar Entidade",
                    TipoUsuario = 0,
                    DataCriacao = new DateTime(2023, 10, 5, 0, 0, 0, DateTimeKind.Local),
                    Deletado = false
                },
                new Permissao()
                {
                    Id = new Guid(),
                    Nome = "DeletarEntidade",
                    Descricao = "Deletar Entidade",
                    TipoUsuario = 0,
                    DataCriacao = new DateTime(2023, 10, 5, 0, 0, 0, DateTimeKind.Local),
                    Deletado = false
                }

            #endregion

            // ...
            );
        }
    }
}
```

## alterar GetPermissoesQueryHandler

```csharp
namespace Core.Queries.Security.Handler
{
    public class GetPermissoesQueryHandler : IRequestHandler<GetPermissoesQuery, Result<IEnumerable<PermissaoResponse>>>
    {
        private readonly IPermissaoRepository _repository;
        private readonly IMapper _mapper;
        
        public GetPermissoesQueryHandler(IPermissaoRepository repository, 
            IMapper mapper)
        {
            _repository = repository;
            _mapper = mapper;
        }
        public async Task<Result<IEnumerable<PermissaoResponse>>> Handle(GetPermissoesQuery query, CancellationToken cancellationToken)
        {
            var result = new Result<IEnumerable<PermissaoResponse>>();
                
            var permissoes = (await _repository.Get()).ToList();
            IEnumerable<Entities.Security.Permissao> permissaoModulo = new List<Entities.Security.Permissao>();

            foreach (var item in query.Filter.Modulos)
            {
                if (item == "Nm") permissaoModulo = permissaoModulo.Concat(permissoes.Where(p => p.Nome.Contains("NM")));
                else if (item == "Rdo") permissaoModulo = permissaoModulo.Concat(
                    permissoes.Where(p => p.Nome.Contains("Ocorrencia") || p.Nome.Contains("Area") || p.Nome.Contains("Evento") || p.Nome.Contains("Local") || p.Nome.Contains("Gerencia")));
                else if (item == "Inventario") permissaoModulo = permissaoModulo.Concat(
                    permissoes.Where(p => p.Nome.Contains("Deposito") || p.Nome.Contains("Planilha") || p.Nome.Contains("Jornada") ||
                    p.Nome.Contains("Inventario") || p.Nome.Contains("Material") || p.Nome.Contains("UnidadeMedida") && !(p.Nome.Contains("NM"))));
                else if (item == "Configuracao") permissaoModulo = permissaoModulo.Concat(
                    permissoes.Where(p => p.Nome.Contains("UnidadeAcesso") || p.Nome.Contains("Usuario") || p.Nome.Contains("UsuarioGrupo") ||
                    p.Nome.Contains("Grupo") || p.Nome.Contains("PermissoesGrupo") || p.Nome.Contains("PermissoesUsuario")));
                else if (item == "Checklist") permissaoModulo = permissaoModulo.Concat(
                    permissoes.Where(p => p.Nome.Contains("TipoEquipamento") || p.Nome.Contains("Equipamento") || p.Nome.Contains("Turno") ||
                    p.Nome.Contains("Checklist") || p.Nome.Contains("Item") || p.Nome.Contains("ModeloChecklist")));
                else if (item == "Contentores") // <-- novo Modulo
                    permissaoModulo = permissaoModulo.Concat(permissoes.Where(p => 
                        p.Nome.Contains("Inspecao") || 
                        p.Nome.Contains("Equipamento") || 
                        p.Nome.Contains("Quadra") ||
                        p.Nome.Contains("Rua") ||
                        p.Nome.Contains("Manutencao") ||
                        p.Nome.Contains("Checklist") // <-- nova linha dentro do modulo
                    ));
            }

            result.Value = permissaoModulo.Select(p => _mapper.Map<PermissaoResponse>(p));

            return result;
        }
    }
}
```

---

[`◀️ anterior`](./10%20Mapper.md) [`próximo ▶️`](./12%20Passos%20Finais.md)
