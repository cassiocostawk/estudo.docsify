<font face="Calibri">

# 🔐 Politica de Segurança

[`⬆️ inicio`](../../Readme.md)

[`◀️ voltar`](../Readme.md)

---

*ou Policy*

*Toda essa funcionalidade não faz parte do Identity*

## 🔐 Adicionando uma Policy a uma Action

+ Adicionar um política de segurança usando `AddAuthorization()`:

```csharp
// adicionar a politica de segurança
services.AddAuthorization(options =>
    options.AddPolicy("IdadeMinima", policy =>
    {
        policy.Requirements.Add(new IdadeMinimaRequirement(18));
    })
);

// injeção do serviço de autorização do handler
services.AddSingleton<IAuthorizationHandler, IdadeMinimaHandler>();
```

+ Exemplo de Política de Segurança no `Controller`:

```csharp
[HttpGet]
[Authorize(Roles = "admin, regular", Policy = "IdadeMinima")] // <--
public IActionResult GetAll([FromQuery] int? classificacaoEtaria = null)
{
    List<ReadFilmeDto> readDto = _filmeService.GetAll(classificacaoEtaria);
    if (readDto != null) return Ok(readDto);
    return NotFound();
}
```

+ Criando a classes de `Requirement` que especifica basicamente a estrutura da classe de política de segurança:

```csharp
public class IdadeMinimaRequirement : IAuthorizationRequirement
{
    public int IdadeMinima { get; set; }

    public IdadeMinimaRequirement(int idadeMinima)
    {
        IdadeMinima = idadeMinima;
    }
}
```

+ Criando a classe de `Handler` onde fica a regra que será acionada ao acessar a Action com tal política:

```csharp
public class IdadeMinimaHandler : AuthorizationHandler<IdadeMinimaRequirement> // Bindind com o Requirement
{
    // Implementando método abstrato HandleRequirementAsync da classe AuthorizationHandler
    protected override Task HandleRequirementAsync(AuthorizationHandlerContext context, IdadeMinimaRequirement requirement)
    {
        // usuario não tem a Claim DateOfBirth 
        if (!context.User.HasClaim(c => c.Type == ClaimTypes.DateOfBirth))
            return Task.CompletedTask; // finaliza a tarefa sem autorizar

        // Pega do usuario do contexto do Token com a Claim de DateOfBirth e converte para DateTime
        DateTime dataNascimento = Convert.ToDateTime(context.User.FindFirst(c =>
            c.Type == ClaimTypes.DateOfBirth)
            .Value);

        int idadeObtida = DateTime.Today.Year - dataNascimento.Year;

        // usuario não fez aniversário no ano ainda
        // data de nascimento é maior que hoje menos idade obtida acima, subtrai um ano
        if (dataNascimento > DateTime.Today.AddYears(-idadeObtida))
            idadeObtida--;

        // tem idade minima, sucesso
        if (idadeObtida >= requirement.IdadeMinima) 
            context.Succeed(requirement);

        return Task.CompletedTask;
    }
}
```

---

[`^ topo`](#Dev)
</font>
