<font face="Calibri">

# 🔐 Redefinir Senhas

[`⬆️ inicio`](../../Readme.md)

[`◀️ voltar`](../Readme.md)

---

## 👣 Passo a Passo

Para redefinir a senha em nosso projeto, necessitamos do token como parâmetro para a chamada do método `ResetPasswordAsync(IdentityUser, Token, NewPassword)` responsável por tal ação.
Esse token é gerado pelo método `GeneratePasswordResetTokenAsync(IdentityUser)`;

----

## 🔑 Token

O papel do token é garantir que a pessoa que irá redefinir a senha é a mesma que solicitou a redefinição.

---

## 🔐 Ataques de Força Bruta

O Identity provê uma alternativa para bloquear tentativas suspeitas de login durante a execução do método de autenticação.
Mais informações sobre como implementar esta funcionalidade podem ser adquiridas através da [documentação](https://docs.microsoft.com/en-us/aspnet/core/security/authentication/identity-configuration?view=aspnetcore-5.0).

---

## 📑 Exemplo

+ No ``Controller`` do Login, criando as rotas:

```csharp
[HttpPost("/solicita-reset")]
public IActionResult SolicitaResetSenhaUsuario(SolicitaResetRequest request)
{
    Result resultado = _loginService.SolicitaResetSenhaUsuario(request);
    if (resultado.IsFailed) return Unauthorized(resultado.Errors);
    return Ok(resultado.Successes);
}

[HttpPost("/efetua-reset")]
public IActionResult ResetaSenhaUsuario(EfetuaResetRequest request)
{
    Result resultado = _loginService.ResetaSenhaUsuario(request);
    if (resultado.IsFailed) return Unauthorized(resultado.Errors);
    return Ok(resultado.Successes);
}
```

+ No `Service` do Login, criando a regra:

```csharp
/* Método que solicita o Reset e retorna um Token de redefinição de senha */
public Result SolicitaResetSenhaUsuario(SolicitaResetRequest request)
{
    IdentityUser<int> identityUser = RecuperaUsuarioPorEmail(request.Email);

    if (identityUser != null)
    {
        string codigoRecuperacao = _signInManager
            .UserManager
            .GeneratePasswordResetTokenAsync(identityUser)
            .Result;

        return Result.Ok().WithSuccess(codigoRecuperacao);
    }

    return Result.Fail("Falha ao solicitar redefinição.");
}

/* Método que Reseta a senha do usuario, permitindo informar uma nova senha */
public Result ResetaSenhaUsuario(EfetuaResetRequest request)
{
    IdentityUser<int> identityUser = RecuperaUsuarioPorEmail(request.Email);

    IdentityResult resultadoIdentity = _signInManager
        .UserManager
        .ResetPasswordAsync(identityUser, request.Token, request.Password)
        .Result;

    if (resultadoIdentity.Succeeded) return Result.Ok()
            .WithSuccess("Senha redefinida com sucesso!");

    return Result.Fail("Falha ao redefinir a senha!");
}

/* Recuperar usuario usado acima */
private IdentityUser<int> RecuperaUsuarioPorEmail(string email)
{
    return _signInManager
        .UserManager
        .Users
        .FirstOrDefault(u => u.NormalizedEmail == email.ToUpper());
}
```

+ Classes de DTO utilizadas e suas anotações importantes:

```csharp
public class SolicitaResetRequest
{
    [Required]
    public string Email { get; set; }
}

public class EfetuaResetRequest
{
    [Required]
    [DataType(DataType.Password)]
    public string Password { get; set; }
    [Required]
    [DataType(DataType.Password)]
    [Compare("Password")] // verifica equiparidade das senhas
    public string RePassword { get; set; }
    [Required]
    public string Email { get; set; }
    [Required]
    public string Token { get; set; }
}
```

---

[`^ topo`](#Dev)
</font>
